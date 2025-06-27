<script>
import Button from './button.vue';

export default {
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
        <template v-if="sortingOrder.number === orderNumberToChange">{{ sortingOrder.ascending ? "&#128897" : "&#128899" }}                            
        </template>
        <div v-else class="arrow-stack">
            <div>&#128897</div>
            <div>&#128899</div>  
        </div>
    </Button>
</template>

<style>
.arrow-stack {
  line-height: 1px;
}
</style>