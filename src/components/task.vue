<script>
export default{
    props:{
        isDark:Boolean,
        thisTask:Object,
    }
}
</script>

<template>
    <div class="task-el">
        <div class="task-top">
            <div class="margin-left text-overflow"><h3 >{{ thisTask.title || "-No Titel-"  }}</h3></div>
            <div class="horizontal-line"></div>                
        </div>              
        <p class="textblock-overflow">{{ thisTask.body || "-No Body-" }}</p>
    </div>
</template>

<style>

.dark .task-el {
  background-color: #2e2e34;
  box-shadow: 0px 0px 8px 2px rgb(0, 0, 0);
  color:#fff
}
.task-el {
  background-color: #fff;
  height: 140px;
  padding: 5px;
  border-radius: 5px;
  border-top-left-radius: 40px 40px;
  border-bottom-right-radius: 40px 40px;
  box-shadow: 0px 0px 8px 2px rgb(92, 122, 132);
}
.horizontal-line {
  height: 1px;
  background-color: rgb(0, 0, 0);
  width: 50%;
  align-self: center;
}
.dark .horizontal-line {
  background-color: white;
}
.textblock-overflow {
  padding-left: 10px;
  padding-right: 10px;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.margin-left {
  margin-left: 20px;
}
.text-overflow {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.task-top {
    display: flex;
    flex-direction: column;    
    justify-content: space-between;
}
</style>