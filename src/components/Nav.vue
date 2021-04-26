<template>
    <div class="container-fluid">
        <div class="row align-items-center">
            <div class="col-sm-1 col-lg-1"></div>
            <div class="col-12 col-sm-4 col-lg-2">
                <img class="w-100" src="../assets/Rick_and_Morty.png">
            </div>
            <div class="col-12 col-sm-6 col-lg-7">
                <div class="container-fluid">
                    <div class="row justify-content-start">
                        <div class="col col-sm col-md-10 col-lg-7 col-xl-6 align-items-left border">
                            <div class="container">
                                <div class="row align-items-start">
                                    <div class="col-3 bg-dark search">
                                        Search by
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-sm-1 col-lg"></div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue' 

export default defineComponent({
  props: [],
  components: {},
  computed: {}
})
</script>

<style>

.marginZero {
    margin-left: 0;
    padding-left: 0;
}

.search {
    color: #A9B1BD;
    min-width: 95px;
    border-right: 1px #A9B1BD solid;
    margin-left: 0;
}

.border{
    border-width: 5px; 
    border-color: #A9B1BD;
    border-radius: 15px;
    text-align: center;
}

.searchBar {
    color: #A9B1BD;
    border: 2px solid #A9B1BD;
    border-radius: 5px;
    height: 56px;
}

</style>