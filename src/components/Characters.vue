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
            <div class="col-12 col-sm-1">
                <div class="container-fluid">
                    <div class="row align-items-start position-relative">
                        <img class="col-12 charImage" :class="{greyImg: char.status === 'Dead'}" :src="char.image">
                        <img class="ribbon" v-if="char.status === 'Dead'" src="../assets/ribbon.png">
                    </div>
                </div>
            </div>
            <div class="col-12 col-sm text-left">{{char.id}}</div>
            <div class="col-12 col-sm text-left">{{char.name}}</div>
            <div class="col-12 col-sm text-left">
                <div class="container-fluid" style="margin-left: -18px;">
                    <div class="row align-items-center justify-content-center">
                        <img v-if="char.gender == 'Male'"               class="col-3 genderIcon" src="../assets/male.png">
                        <img v-else-if="char.gender == 'Female'"        class="col-3 genderIcon" src="../assets/female.png">
                        <img v-else-if="char.gender == 'Genderless'"    class="col-3 genderIcon" src="../assets/clear.png">
                        <img v-else                                     class="col-3 genderIcon" src="../assets/unknown.png">
                        <div class="col text-left" style="margin-left: -20px;">{{char.gender}}</div>
                    </div>
                </div>
            </div>
            <div class="col-12 col-sm text-left">{{char.species}}</div>
            <div class="col-12 col-sm text-left">{{getEpisode(char.episode)}}</div>
            <div class="col-sm-1 container-fluid">
                <div class="row align-items-start">
                    <img v-if="char.favorite" @click="$emit('removeFavorite', char)" class="photo col-1 col-sm-5" src="../assets/star2.png">
                    <img v-else @click="$emit('addFavorite', char)" class="photo col-1 col-sm-5" src="../assets/star.png">
                </div>
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
    methods: {
        getEpisode(episodes: Array<any>): string {
            if (!episodes || episodes.length < 1) return "";
            else return episodes[episodes.length-1].episode;
        },
        genderIconUri(gender: any): string {
            console.log('gender:', gender);
            switch(gender) {
                case 'Male':
                    return "../assets/male.png";
                case 'Female':
                    return "../assets/female.png";
                case 'Genderless':
                    return "../assets/clear.png";
                default:
                    return "../assets/unknown.png";
            }
        }
    }
})
</script>

<style scoped>
@media (max-width: 750px) {
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
    min-width: 80px;
    max-width: 300px;
}

.charImage {
    min-height: 90px;
    min-width: 80px;
    margin-left: -15px;
}

.greyImg {
    filter: grayscale(1);
    opacity: 0.98;
}
.ribbon {
    min-width: 35px;
    max-width: 60px;
    position: absolute;
    top: -10px;
    right: 12px;
}

.genderIcon {
    min-width: 55px;
    max-width: 55px;
}
</style>