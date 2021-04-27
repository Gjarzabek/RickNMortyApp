<template>
  <Nav/>
  <Menu @optionChange="ChangeDisplay"/>
  <Characters @addFavorite="addFavoriteChar" @removeFavorite="removeFavoriteChar" :characters="currentPayload"/>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Nav from "./components/Nav.vue";
import Menu from "./components/Menu.vue";
import Characters from "./components/Characters.vue";

@Options({
  components: {
    Nav,
    Menu,
    Characters
  },
  data() {
    return {
      display: "All Characters",
      currentPage: [
        {"id":22,"name":"Aqua Rick","status":"unknown","species":"Humanoid","type":"Fish-Person","gender":"Male","origin":{"name":"unknown","url":""},"location":{"name":"Citadel of Ricks","url":"https://rickandmortyapi.com/api/location/3"},"image":"https://rickandmortyapi.com/api/character/avatar/22.jpeg","episode":["https://rickandmortyapi.com/api/episode/10","https://rickandmortyapi.com/api/episode/22","https://rickandmortyapi.com/api/episode/28"],"url":"https://rickandmortyapi.com/api/character/22","created":"2017-11-04T22:41:07.171Z"},
        {"id":222,"name":"Michael Denny and the Denny Singers","status":"Alive","species":"Human","type":"","gender":"Male","origin":{"name":"unknown","url":""},"location":{"name":"Interdimensional Cable","url":"https://rickandmortyapi.com/api/location/6"},"image":"https://rickandmortyapi.com/api/character/avatar/222.jpeg","episode":["https://rickandmortyapi.com/api/episode/8"],"url":"https://rickandmortyapi.com/api/character/222","created":"2017-12-30T14:44:05.245Z"},
        {"id":574,"name":"Snake Lincoln","status":"Dead","species":"Animal","type":"Snake","gender":"Male","origin":{"name":"Snake Planet","url":"https://rickandmortyapi.com/api/location/78"},"location":{"name":"Snake Planet","url":"https://rickandmortyapi.com/api/location/78"},"image":"https://rickandmortyapi.com/api/character/avatar/574.jpeg","episode":["https://rickandmortyapi.com/api/episode/36"],"url":"https://rickandmortyapi.com/api/character/574","created":"2020-05-07T12:13:44.361Z"},
        {"id":622,"name":"Sarge","status":"Alive","species":"Human","type":"Soulless Puppet","gender":"Male","origin":{"name":"Story Train","url":"https://rickandmortyapi.com/api/location/96"},"location":{"name":"Story Train","url":"https://rickandmortyapi.com/api/location/96"},"image":"https://rickandmortyapi.com/api/character/avatar/622.jpeg","episode":["https://rickandmortyapi.com/api/episode/37"],"url":"https://rickandmortyapi.com/api/character/622","created":"2020-08-06T15:59:44.277Z"},
        {"id":368,"name":"Truth Tortoise","status":"unknown","species":"Mythological Creature","type":"Omniscient being","gender":"Male","origin":{"name":"unknown","url":""},"location":{"name":"unknown","url":""},"image":"https://rickandmortyapi.com/api/character/avatar/368.jpeg","episode":["https://rickandmortyapi.com/api/episode/29"],"url":"https://rickandmortyapi.com/api/character/368","created":"2018-01-10T19:14:35.885Z"},
        {"id":6,"name":"Abadango Cluster Princess","status":"Alive","species":"Alien","type":"","gender":"Female","origin":{"name":"Abadango","url":"https://rickandmortyapi.com/api/location/2"},"location":{"name":"Abadango","url":"https://rickandmortyapi.com/api/location/2"},"image":"https://rickandmortyapi.com/api/character/avatar/6.jpeg","episode":["https://rickandmortyapi.com/api/episode/27"],"url":"https://rickandmortyapi.com/api/character/6","created":"2017-11-04T19:50:28.250Z"},
        {"id":16,"name":"Amish Cyborg","status":"Dead","species":"Alien","type":"Parasite","gender":"Male","origin":{"name":"unknown","url":""},"location":{"name":"Earth (Replacement Dimension)","url":"https://rickandmortyapi.com/api/location/20"},"image":"https://rickandmortyapi.com/api/character/avatar/16.jpeg","episode":["https://rickandmortyapi.com/api/episode/15"],"url":"https://rickandmortyapi.com/api/character/16","created":"2017-11-04T21:12:45.235Z"},
        {"id":25,"name":"Armothy","status":"Dead","species":"unknown","type":"Self-aware arm","gender":"Male","origin":{"name":"Post-Apocalyptic Earth","url":"https://rickandmortyapi.com/api/location/8"},"location":{"name":"Post-Apocalyptic Earth","url":"https://rickandmortyapi.com/api/location/8"},"image":"https://rickandmortyapi.com/api/character/avatar/25.jpeg","episode":["https://rickandmortyapi.com/api/episode/23"],"url":"https://rickandmortyapi.com/api/character/25","created":"2017-11-05T08:54:29.343Z"}
      ],
      favorites: []
    };
  },
  methods: {
      ChangeDisplay(event: string): void {
        if (event === this.display) return;
        this.display = event;
        console.log("changeDisplay: ", event);
      },
      addFavoriteChar(character: any): void {
        character.favorite = true;
        this.favorites.push(character);
      },
      removeFavoriteChar(charId: number): void {
        const character = this.favorites.find((char: any) => {
            return char.id === charId;
        });
        if (!character) return;
        character.favorite = false;
        this.favorites = this.favorites.filter((char: any) => {return char.id != charId});
      }
  },
  computed: {
    currentPayload: function(): Array<any> {
      if (this.display === "All Characters") return this.currentPage;
      else if (this.display === "Favorites") return this.favorites;
      else return [];
    }
  }
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
</style>
