<template>
  <Nav @filterChange="changeFilter" @search="searchRequest"/>
  <Menu @optionChange="ChangeDisplay" :active="activeMenu"/>
  <Characters @addFavorite="addFavoriteChar" @removeFavorite="removeFavoriteChar" :characters="currentPayload"/>
  <PagesIterator :startPage="pageNum" v-if="isDisplayAllChars" @newPage="changePage"/>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Nav from "./components/Nav.vue";
import Menu from "./components/Menu.vue";
import Characters from "./components/Characters.vue";
import PagesIterator from "./components/PagesIterator.vue";

@Options({
  components: {
    Nav,
    Menu,
    Characters,
    PagesIterator
  },
  data() {
    return {
      display: "All Characters",
      Characters: new Map(), // pageNum -> [Characters]
      searchResult: [],
      favorites: [],
      filter: "Page",
      pageNum: 1,
      graphQlUri: 'https://rickandmortyapi.com/graphql',
    };
  },
  created() {
      this.searchRequest(1);
      this.filter = "Name";
      const favoritesString: any = localStorage.getItem('favorites');
      try {
        this.favorites = JSON.parse(favoritesString);
      }
      catch {
        this.favorites = [];
      }
      if (!this.favorites)  this.favorites = [];
      for (const character of this.favorites)
        character.favorite = true;
  },
  methods: {
      markFavorites(characters: Array<any>): void {
        if (!this.favorites)  this.favorites = [];
        for (const character of characters) {
            const isInFavorites = this.favorites.find((favorite: any) => {
                return favorite.id === character.id;
            });
            if (isInFavorites) {
              character.favorite = true;
            }
        }
      },
      ChangeDisplay(event: string): void {
        this.display = event;
      },
      addFavoriteChar(character: any): void {
        const isInFavorites = this.favorites.find((char: any) => {
            return char.id === character.id;
        });
        character.favorite = true;
        if (isInFavorites)  return;
        this.favorites.push(character);
        localStorage.setItem('favorites', JSON.stringify(this.favorites));
        this.updateCharacterMap(character);
      },
      removeFavoriteChar(eventChar: any): void {
        const charId: number = eventChar.id;
        const character = this.favorites.find((char: any) => {
            return char.id === charId;
        });
        if (!character) return;
        character.favorite = false;
        eventChar.favorite = false;
        this.favorites = this.favorites.filter((char: any) => {return char.id != charId});
        localStorage.setItem('favorites', JSON.stringify(this.favorites));
        this.updateCharacterMap(character);
      },
      async updateCharacterMap(character: any): Promise<void> {
        for (const pair of this.Characters) {
            for (const char of pair[1])
              if (char.id === character.id) char.favorite = character.favorite;
        }
      },
      changePage(pageNum: number): void {
        this.pageNum = pageNum;
        this.filter = "Page";
        if (!this.Characters.has(pageNum)) {
          this.Characters.set(pageNum, []);
          this.searchRequest(pageNum);
        }
      },
      changeFilter(filter: string): void {
        this.filter = filter;
      },
      searchRequest(phrase: string | number): void {
        if (this.filter !== 'Page') this.ChangeDisplay('Search')
        else                        this.ChangeDisplay('All Characters')

        switch (this.filter) {
          case 'Name':
              fetch(this.graphQlUri, {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({
                query: `
                  query {
                    characters(filter:{name:"${phrase}"}) {
                      results{     
                        id
                        name
                        gender
                        status
                        species
                        image
                        episode {
                          episode
                        }
                      }
                    }
                  }
                `
              })
            }).then(res => res.json()).then(data => {
              if(!data.data.characters) return;
              this.markFavorites(data.data.characters.results);
              this.searchResult = data.data.characters.results;
            });
          break;
          case 'Identifier':
              fetch(this.graphQlUri, {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({
                query: `
                  query {
                    character(id:${phrase}) {
                        id
                        name
                        gender
                        status
                        species
                        image
                        episode {
                          episode
                        }
                    }
                  }
                `
              })
            }).then(res => res.json()).then(data => {
              if (!data.data.character) return;
              this.searchResult = [data.data.character];
              this.markFavorites(this.searchResult);
            });
          break;
          case 'Episode':
              fetch(this.graphQlUri, {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({
                query: `
                  query {
                    episodes(filter:{episode:"${phrase}"}) {
                      results {
                        characters{     
                          id
                          name
                          gender
                          status
                          species
                          image
                          episode {
                            episode
                          }
                        }
                      }
                    }
                  }
                `
              })
            }).then(res => res.json()).then(data => {
              if (!data.data.episodes) return;
              this.searchResult = data.data.episodes.results[0].characters
              this.markFavorites(data.data.episodes.results[0].characters);
            });
          break;
          case 'Page':
            fetch(this.graphQlUri, {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({
                query: `
                  query {
                    characters(page:${this.pageNum}) {
                      results {
                        id
                        name
                        gender
                        status
                        species
                        image
                        episode {
                          episode
                        }
                      }
                    }
                  }
                `
              })
            }).then(res => res.json()).then(data => {
              if (!data.data.characters)  return;
              this.Characters.set(phrase, data.data.characters.results);
              this.markFavorites(data.data.characters.results);
            });
          break;
        }
      }
  },
  computed: {
    currentPayload: function(): Array<any> {
      if (this.display === "All Characters") return this.Characters.get(this.pageNum);
      else if (this.display === "Favorites") return this.favorites;
      else if (this.display === "Search") return this.searchResult;
      else return [];
    },
    isDisplayAllChars: function(): boolean {
      return this.display === "All Characters";
    },
    activeMenu: function(): boolean {
      return this.display !== "Search";
    }
  }
})
export default class App extends Vue {}
</script>

<style>
@font-face {
    font-family: Poppins;
    src: url(assets/Poppins-Regular.ttf);
}

#app {
  font-family: Poppins;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
</style>
