<script>
import { columnsTitleJS } from '@/data/columns';
import Task from './task.vue';
import { columnsJS } from '@/data/columns';

export default{   
    data(){
    return {
      columns:columnsJS,
      columnsTitle:columnsTitleJS,      
    }
  },

   components: {
    Task,
   },

    props: {
        isDark:Boolean,
        handlePlusClick: Function,
        handleDarkClick: Function,
        tasks: Array,
        returnTask:Function,
        onTaskDrop: Function,
        startDrag: Function,
        handleTitleClick: Function,
        onDropColumn: Function,
    }
}
</script>

<template>
    <table class="kanban-table">      
      <thead>
        <tr>
          <td v-for="title in columnsTitle" :key="title" class="title">{{ title }}</td>
        </tr>
      </thead>
      <tbody>
        <tr  draggable="false">
          <td v-for="colum in columns" :key="colum" class="drop-zone" @drop="onDropColumn($event, colum)" @dragover.prevent @dragenter.prevent>
            <div v-for="task in returnTask(colum)" :key="task.id" >
            <div class="task-drop-zone" @drop="onDropBetweenTasks($event, task)" @dragover.prevent @dragenter.prevent></div>
              <Task class="task-el" :thisTask="task" draggable="true" @click ="handleTitleClick(task)" @dragstart="startDrag($event, task)" @drop="onTaskDrop($event, task)" @dragover.prevent @dragenter.prevent></Task>
            </div>
            <div class="task-bottom-margin" @drop="onDropColumn($event, colum)" @dragover.prevent @dragenter.prevent></div>
          </td>
      </tr>
      <tr>
          <td v-for="title in columnsTitle" :key="title" class="bottom"> - </td>
        </tr>
      </tbody>
    </table>
</template>

<style>
.kanban-table{
  table-layout: fixed;
  width:100%; 
  border-collapse: collapse;
  margin-left: 2%;
  margin-right: 2%;
}
.title{
  font-size:x-large;
  font-weight: bold;
  color:white;
  background-color: rgb(0, 57, 82);
  border-top-left-radius: 80px 80px;
  border-top-right-radius: 20px 20px;
}
.dark .title{
  color:white;
  background-color: rgb(31, 37, 39);
}
.task-drop-zone {
  min-height: 18px;
}
.drop-zone {
  background-color: #ffffff;
  height: 600px;
  min-width: 160px;
  margin-bottom: 10px;
  padding-left: 10px;
  padding-right: 10px;
  vertical-align: top;
  border-left: 1px solid black;
  border-right: 1px solid black;
}
.dark .drop-zone {
  background-color: rgb(76, 82, 88);
}
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
.task-bottom-margin {
  min-height: 18px;

}
.dark .bottom{
  color:white;
  background-color: rgb(31, 37, 39);
}
.bottom{
  font-size:x-large;
  font-weight: bold;
  color:white;
  background-color: rgb(0, 57, 82);
  border-bottom-right-radius: 80px 80px;
  border-bottom-left-radius: 20px 20px;
}

td{
  color: rgb(0, 0, 0);
  text-align: center;
}
h3{
  font-weight: bold;
}
</style>