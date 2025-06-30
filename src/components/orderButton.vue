<script>
import Button from './button.vue';

export default {
    data(){
        return {
            up: "^",
            down: "v",
        }
    },
    props: {
        changeSortingOrder:Function,
        sortingOrder:Object,
        orderNumberToChange:Number,
    },
    components: {
        Button,
    }
}
</script>

<template>
    <Button @click="changeSortingOrder(orderNumberToChange)" :button-size="20">
        <template v-if="sortingOrder.number === orderNumberToChange">{{ sortingOrder.ascending ? up : down }}                            
        </template>
        <div v-else class="arrow-stack">
            <div>{{ this.up }}</div>
            <div>{{ this.down }}</div>
        </div>
    </Button>
</template>

<style>
.arrow-stack {
  line-height: 5px;
}
</style>