<template>
    <div class="container-fluid">
        <div class="row align-items-center bottomBorder p-4" id="firstrow">
            <b class="col-1 text-left"></b>
            <b class="col-1 text-left" style="width: 50px;">Photo</b>
            <b class="col text-left">Character ID</b>
            <b class="col text-left">Name</b>
            <b class="col text-left">Gender</b>
            <b class="col text-left">Species</b>
            <b class="col text-left">Last Episode</b>
            <b class="col-1 text-nowrap text-left">Add To Favorites</b>
            <b class="col-1 text-left"></b>
        </div>
        <div v-for="char in characters" v-bind:key="char.id" class="row align-items-center p-4 bottomBorder">
            <div class="col-1 text-left"></div>
            <img class="col-12 col-sm-1 text-left" :src="char.image">
            <div class="col-12 col-sm text-left">{{char.id}}</div>
            <div class="col-12 col-sm text-left">{{char.name}}</div>
            <div class="col-12 col-sm text-left">{{char.gender}}</div>
            <div class="col-12 col-sm text-left">{{char.species}}</div>
            <div class="col-12 col-sm text-left">S01E01</div>
            <div class="col-1">
                <img v-if="char.favorite" @click="$emit('removeFavorite', char.id)" class="photo" src="../assets/star2.png">
                <img v-else @click="$emit('addFavorite', char)" class="photo" src="../assets/star.png">
            </div>
            <div class="col-1 text-left"></div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue' 
//<img :src="char.image">
export default defineComponent({
    props: ["characters"],
    components: {},
    computed: {},
    methods: {}
})
</script>

<style scoped>
@media (max-width: 575px) {
    #firstrow {
        display:none;
    }
}

#firstrow {
    background-color: #E5EAF4;
}

.bottomBorder {
    border-bottom: 1px solid #E5EAF4;
    color: #A9B1BD;
}

.photo {
    max-width: 80px;
}
</style>