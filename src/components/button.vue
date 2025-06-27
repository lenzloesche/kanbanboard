<script>
    export default{
        props: {
            buttonSize:Number,
            lightMode:Boolean,
        },
        methods: {
        handleButtonClick() {
            this.$emit('button-click'); 
        }
    }
    }
</script>

<template>
    <button :class="['button', { 'button-light': lightMode }]" :style="{ fontSize: buttonSize + 'px' }" @click="handleButtonClick()"><slot></slot></button>
</template>

<style>
.button {
    background-color: rgb(255, 255, 255);
    color:rgb(0, 0, 0);
    margin: 4px;   
    border-radius: 10px 10px 10px 10px;
    border:2px solid black;
    min-height: 32px;
}
.button-light {
    color:black;
    background-color: rgb(186, 208, 217);
    border: 2px solid black;
}
.button:active {
    background-color: rgb(10, 59, 80);
}
.dark button {
    background-color: rgb(76, 84, 88);
    color: white;
}
</style>