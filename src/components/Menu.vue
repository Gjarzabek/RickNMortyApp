<template>
    <div class="container-fluid">
        <div class="row align-items-left">
            <div class="col-sm-1 col-lg-1"></div>
            <div class="col-5 col-sm-4 col-md-3 col-lg-2 col-xl-1 text-left">
                <div class="menuItem" :class="{color: isActive('All Characters') && active}" @click="signalActive('All Characters')">
                    All Characters
                </div>
            </div>
            <div class="col-5 col-sm-4 col-md-3 col-lg-2 col-xl-1 text-left">
                <div class="menuItem" :class="{color: isActive('Favorites') && active}" @click="signalActive('Favorites')">
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
    props: ["active"],
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
            this.activeOption = option;
            this.$emit('optionChange', option);
        }
    },
    components: {},
    computed: {}
})
</script>

<style scoped>

.menuItem {
    margin: 10px;
    margin-left: 20px;
    text-align: center;
    width: 120px;
    cursor: pointer;
    color: #A9B1BD;
}

.color {
    border-bottom: 4px solid #11B0C8;
    color: #11B0C8;
}

</style>