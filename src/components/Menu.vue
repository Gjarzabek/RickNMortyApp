<template>
    <div class="container-fluid">
        <div class="row align-items-left">
            <div class="col-sm-1 col-lg-1"></div>
            <div class="col-5 col-sm-4 col-md-3 col-lg-2 col-xl-1">
                <div class="debug" :class="{color: isActive('All Characters')}" @click="signalActive('All Characters')">
                    All Characters
                </div>
            </div>
            <div class="col-5 col-sm-4 col-md-3 col-lg-2 col-xl-1">
                <div class="debug" :class="{color: isActive('Favorites')}" @click="signalActive('Favorites')">
                    Favorites
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue' 

type MenuOption = 'All Characters' | 'Favorites';

export default defineComponent({
    props: [],
    data() {
        return {
            activeOption: "All Characters"
        }
    },
    methods: {
        isActive(option: MenuOption): boolean {
            return option === this.activeOption;
        },
        signalActive(option: MenuOption): void {
            if (this.activeOption === option) return;
            this.activeOption = option;
            this.$emit('optionChange', option);
        }
    },
    components: {},
    computed: {}
})
</script>

<style>

.debug {
    margin: 10px;
    text-align: center;
    width: 110px;
}

.color {
    border-bottom: 4px solid #11B0C8;
    color: #11B0C8;
}

</style>