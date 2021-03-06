<template>
    <div class="container-fluid borderBottom">
        <div class="row align-items-center">
            <div class="col-sm-1 col-lg-1"></div>
            <div class="col-12 col-sm-4 col-lg-2">
                <img class="w-100" src="../assets/Rick_and_Morty.png">
            </div>
            <div class="col-12 col-sm-6 col-lg-7">
                <div class="container-fluid">
                    <div class="row justify-content-start">
                        <div class="col-12 col-md-11 col-lg-8 colr-xl-5 align-items-left">
                            <div class="navBar">
                                <div id="searchby" class="navBarItem">Search by</div>
                                <div id="dropdown" class="navBarItem" @click="toogleDropdown">
                                    <div class="itemContent">
                                        {{activeFilter}}
                                        <div class="arrowDown"></div>
                                    </div>
                                </div>
                                <div class="container">
                                    <div class="row">
                                        <input class="col-7 col-sm-7 col-xl-10 searchInput" type="text" v-model="searchPhrase" @keypress="inputKeyPressed" :placeholder="currentPlaceholder" autocomplete="off">
                                        <img class="searchIcon" @click="signalSearch" src="../assets/search.png">
                                    </div> 
                                </div>
                            </div>
                        </div>
                    </div>
                    <div id="dropdownMenu" class="row dropDownMenu" v-if="showDropdown">
                        <div class="col align-items-center" style="margin: 0px; padding: 0px;">
                            <div class="menuItem" @click="setActiveFilter('Name')">Name</div>
                            <div class="menuItem" @click="setActiveFilter('Identifier')">Identifier</div>
                            <div class="menuItem last" @click="setActiveFilter('Episode')">Episode</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue' 

type Filter = "Name" | "Episode" | "Identifier";

export default defineComponent({
  props: [],
  data() {
      return {
          showDropdown: false,
          activeFilter: "Name",
          searchPhrase: ""
      };
  },
  methods: {
      toogleDropdown(): void {
          this.showDropdown = !this.showDropdown;
      },
      setActiveFilter(filter: Filter): void {
          this.toogleDropdown();
          if (this.activeFilter===filter)   return;
          this.activeFilter = filter;
          this.$emit('filterChange', filter);
      },
      inputKeyPressed(event: any): void {
        if (event.key === "Enter") {
            this.signalSearch();
        }
      },
      signalSearch() {
        if (this.searchPhrase.length > 0) {
            this.$emit('filterChange', this.activeFilter);
            this.$emit('search', this.searchPhrase);
            this.searchPhrase = "";
        }
      }
  },
  components: {},
  computed: {
      currentPlaceholder: function(): string {
            switch(this.activeFilter) {
                case 'Name':
                    return "character name";
                case 'Identifier':
                    return "character id eg. 4";
                case 'Episode':
                    return 'episode code eg. S01E01';
                default:
                    return '';
            }
      }
  }
})
</script>

<style scoped>
@media (max-width: 575px) {
    #searchby {
        min-width: 70px;
    }
    #dropdown {
        min-width: 85px;
    }
    #dropdownMenu {
        left: 100px;
    }
}
.borderBottom {
    padding-bottom: 30px;
    border-bottom: 1px solid #E5EAF4;
}

.searchIcon {
    margin-top: 5px;
    height: 34px;
    width: auto;
}

.navBar {
    position: relative;
    border-radius: 10px;
    border: 1px solid #A9B1BD;
    background-color: rgb(255, 255, 255);
    display: flex;
}

.searchInput {
    outline: none;
    border: none;
    min-width: 30px;
    height: 44px;
    color: #A9B1BD;
}

.searchInput::placeholder {
    color: #a9b1bdb7;
}

.navBarItem {
    min-width: 105px;
    padding: 10px;
    color: #A9B1BD;
    border-right: 1px #A9B1BD solid;
}

.dropDownMenu {
    position: absolute;
    left: 135px;
    margin-left: 0;
    z-index: 2000;
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
    padding-left: 0;
}

.menuItem {
    text-align: left;
    width: 106px;
    color: #A9B1BD;
    border-bottom: #A9B1BD 1px solid;
    border-left: #A9B1BD 1px solid;
    border-right: #A9B1BD 1px solid;
    padding: 10px;
    margin: 0;
}

.last {
    border-bottom-right-radius: 10px;
    border-bottom-left-radius: 10px;
}

.itemContent {
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    flex-wrap: wrap;
}

.arrowDown {
  width: 0; 
  height: 0; 
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 8px solid #A9B1BD;
}

</style>