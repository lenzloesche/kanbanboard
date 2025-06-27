<script>
import Button from './button.vue';

export default{   
    
   components: {
    Button,
   },

    props: {
        isDark:Boolean,
        handlePlusClick: Function,
        handleDarkClick: Function,
        handleSwitchClick: Function,
    }
}
</script>

<template>
    <div class="top-flex">
      <h1 class="website-title"> Kanban Board</h1>
      <div class="vertical-line"></div>
      <Button @button-click="handlePlusClick" :buttonSize="30" >+</Button>
      <Button @button-click="handleSwitchClick" :buttonSize="24" class="change-view" >Change View</Button>
      <Button @button-click="handleDarkClick" :buttonSize="30">
        <img v-if="!isDark" src="./../assets/moon_dark.png" height="20" alt="Mond"/>
        <img v-if="!isDark" src="./../assets/sun_dark.png" height="20" alt="Sonne"/>
        <img v-if="isDark" src="./../assets/moon.png" height="20" alt="Mond"/>
        <img v-if="isDark" src="./../assets/sun.png" height="20" alt="Sonne"/>
      </Button>
    </div>
</template>

<style>
.top-flex {
    height: 44px;
    display: flex;
    flex-direction: row;
    background-color: rgb(0, 57, 82);
    border-bottom-left-radius: 80px 80px;
    border-bottom-right-radius: 80px 80px;
    justify-content: flex-start;
    margin-bottom: 8px;    
}

.dark .top-flex{
  color:white;
  background-color: rgb(39, 43, 48);
}

.website-title{
  padding-left: 34px;  
  font-size:xx-large;
  font-weight: bold;
  color:white;
  overflow: hidden;
}

.change-view{
  overflow: hidden;
}

.vertical-line {
  width: 4px;
  margin-left: 14px;
  margin-right: 6px;
  background-color: white;
  align-self: stretch;
}
</style>