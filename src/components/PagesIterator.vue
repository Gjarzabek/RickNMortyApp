<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-1"></div>
            <div class="col">
                <div class="pages">
                    <div class="pageItem" @click="moveBack">
                        <div class="arrowDown arrowLeft"></div>
                    </div>
                    <div v-for="page in pagesToDisplay" v-bind:key="page" class="pageItem" :class="{activePage: isActive(page)}" @click="signalActivePage(page)">{{page}}</div>
                    <div v-if="!dontDisplayDots" class="pageItem">...</div>
                    <div v-if="!dontDisplayLast" class="pageItem" :class="{activePage: isActive(9)}"  @click="jumpToPage(9)">9</div>
                    <div class="pageItem" @click="moveForward">
                        <div class="arrowDown arrowRight blueArrow"></div>
                    </div>
                </div>
            </div>
            <div class="col-1"></div>
        </div>        
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue' 
export default defineComponent({
    props: [],
    data() {
        return {
            pages: [1, 2, 3, 4, 5, 6, 7, 8, 9],
            activePage: 1,
            p: 0, // id of first displayed page
            previewLen: 3
        };
    },
    computed: {
        pagesToDisplay: function(): Array<number> {
            let result: Array<number> = [];
            for (let i = 0; i <= this.previewLen; i++) {
                result.push(this.pages[this.p+i]);
            }
            return result;
        },
        activePageId: function(): number {
            return this.activePage-1;
        },
        dontDisplayDots: function(): boolean {
            return this.p >= 4;
        },
        dontDisplayLast: function(): boolean {
            return this.isActive(9) || this.p > 4;
        }
    },
    methods: {
        signalActivePage(pageNum: number): void {
            if (this.activePage === pageNum)    return;
            this.activePage = pageNum;
            this.$emit('newPage', pageNum);
        },
        isActive(pageNum: number): boolean {
            return pageNum === this.activePage;
        },
        moveForward(): void {
            if (this.activePage === 9) return;
            if (this.activePageId === this.p + this.previewLen)    this.p++;

            if (this.p < this.pages.length) this.signalActivePage(this.activePage+1);
        },
        moveBack(): void {
            if (this.activePage === 1) return;

            if (this.activePageId === this.p) this.p--;

            if (this.p < this.pages.length) this.signalActivePage(this.activePage-1);
            
        },
        jumpToPage(pageNum: number) {
            this.signalActivePage(pageNum);
            this.p = this.activePageId - this.previewLen;
            console.log("ASD", this.p);
        }
    }
})
</script>

<style scoped>
.pageItem {
    width: 40px;
    height: 40px;
    margin: 5px;
    display: flex;
    align-items: center;
    cursor: pointer;
    justify-content: center;
    border-radius: 6px;
    border: 1px solid #A9B1BD;
    color: #A9B1BD;
}

.pages {
    display: flex;
    margin-left: 18px;
    margin-top: 18px;
    margin-bottom: 18px;
}

.arrowDown {
  width: 0; 
  height: 0; 
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 8px solid #A9B1BD;
}

.arrowLeft {
    transform: rotate(90deg);
}

.arrowRight {
    transform: rotate(-90deg);
}

.blueArrow {
    border-top-color: #11B0C8;
}

.activePage {
    color: white;
    background: #11B0C8;
    border-color: #11B0C8;
}
</style>