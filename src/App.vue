<script>
import TaskNew from './components/taskNew.vue';
import Button from './components/button.vue';
import {tasksJS,fakeTaskJS, editTaskJS} from './data/tasks.js';
import Task from './components/task.vue';
import topBar from './components/topBar.vue';
import KanbanTable from './components/kanbanTable.vue';
import ListView from './components/listView.vue';

export default {
  data(){
    return {
      isDark:false,
      showNewTask:false,
      showBoard:true,
      fakeTask:fakeTaskJS,
      editTask:editTaskJS,
      tasks: JSON.parse(JSON.stringify(tasksJS)),
      isPortrait: false,   
      dragID:-1,
    }
  },

  components: {
    TaskNew,
    Task,
    Button,
    topBar,
    KanbanTable,
    ListView,
  },
  mounted() {
    const saved = localStorage.getItem('kanBanTasks');
    this.tasks = saved ? JSON.parse(saved) : this.tasks;
    const savedDarkMode = localStorage.getItem('kanBanDarkMode');
    this.isDark = savedDarkMode ? JSON.parse(savedDarkMode) : this.isDark;
    this.screenOrientation(window.screen);
    window.addEventListener("orientationchange", () => {
      this.screenOrientation(window.screen);

    });

  },
  watch: {
      tasks: {
        handler(newList) {
          localStorage.setItem('kanBanTasks', JSON.stringify(newList));
        },
        deep: true
      },

    isDark: {
        handler(newList) {
          localStorage.setItem('kanBanDarkMode', JSON.stringify(newList));
        },
        deep: true
      },

      showNewTask(newVal) {
        if (newVal) {
          this.$nextTick(() => {
            const rotated = document.querySelector('.main-rotated');
            if (rotated) {
              rotated.scrollLeft = 0;
              rotated.scrollTop = 0;
            }
          });
        }
      },
      
    },
    
    methods: {
       
      screenOrientation(screen) {
            if (screen.orientation && screen.orientation.type) {
              if (screen.orientation.type.startsWith("portrait")) {
                this.isPortrait = true;
              } else {
                this.isPortrait = false;
              }
            }
          },

      returnShortTitle(title){
          if (title.length>16){
            return title.slice(0,16)+"...";
          }
          return title;
        },

    returnShortBody(body){
          if (body.length>130){
            return body.slice(0,130)+"...";
          }
          return body;
        },
      
    returnTask(number) {
          return this.tasks.filter((task) => task.column === number).sort((a,b) => a.order - b.order);
    },

    startDrag(evt, task) {
      evt.dataTransfer.dropEffect = 'move';
      evt.dataTransfer.effectAllowed = 'move';
      this.dragID = task.id;
    },

    changeTaskPosition(taskToChangeID,ColumnToChangeTo,orderToChangeTo){
      const taskToChangeIndex = this.tasks.findIndex((task)=>task.id===taskToChangeID);
      this.tasks[taskToChangeIndex].column = ColumnToChangeTo;
      this.tasks[taskToChangeIndex].order = orderToChangeTo;
    },

    normalizeColumnOrder(column) {
      const tasksInColumn = this.tasks
        .filter(task => task.column === column)
        .sort((a, b) => a.order - b.order);

      tasksInColumn.forEach((task, index) => {
        task.order = index;
      });
    },


    onTaskDrop(evt, taskDroppedOn) {      
      this.changePosition(evt,taskDroppedOn, "dropOnTask");
    },

    onDropBetweenTasks(evt, taskDroppedOn) {
      if (evt.target.closest('.task-el')) return;
      this.changePosition(evt, taskDroppedOn,"dropBetween");
    },

    onDropColumn(evt, column) {
      if (evt.target.closest('.task-el')) return;
      if (evt.target.closest('.task-drop-zone')) return;
      const fakeTaskDroppedOn = {...this.fakeTask};
      fakeTaskDroppedOn.column=column;
      const taskID = this.dragID;
      const taskToDrop = this.tasks.find((task) => task.id == taskID);
      const tasksInColumnSorted = this.tasks.filter((task) => task.column == fakeTaskDroppedOn.column && task.id != taskToDrop.id).sort((a,b)=>b.order-a.order);
 
      if (tasksInColumnSorted[0]){
        fakeTaskDroppedOn.order=tasksInColumnSorted[0].order + 1;
      }else{
        fakeTaskDroppedOn.order=1;
      }
      this.changePosition(evt, fakeTaskDroppedOn, "dropEnd");      
    },

    changePosition(evt, taskDropped, dropWhere) {
      const taskDroppedOn = {...taskDropped};
      const taskID = this.dragID;
      const taskToDrop = {...this.tasks.find((task) => task.id == taskID)};
      if (taskToDrop.id!=taskDroppedOn.id)
        {          
          const tasksInColumnSorted = this.tasks.filter((task) => task.column == taskDroppedOn.column && task.id != taskToDrop.id).sort((a,b)=>a.order-b.order);
          if (taskDroppedOn.column==taskToDrop.column){
            if (taskDroppedOn.order > taskToDrop.order){
              for (let i=0; i < tasksInColumnSorted.length; i++){
                  const currentTask=tasksInColumnSorted[i];
                  if (currentTask.order<=taskDroppedOn.order && currentTask.order>taskToDrop.order){
                    this.changeTaskPosition(currentTask.id,currentTask.column,currentTask.order-1);
                }
              }                            
            }else if (taskDroppedOn.order < taskToDrop.order){
                for (let i=0;i<tasksInColumnSorted.length;i++){
                  const currentTask=tasksInColumnSorted[i];
                  if (currentTask.order>=taskDroppedOn.order && currentTask.order<taskToDrop.order){
                    this.changeTaskPosition(currentTask.id,currentTask.column,currentTask.order+1);
                }
              }
            }
          }else {
             for (let i = 0; i < tasksInColumnSorted.length; i++){
                  const currentTask=tasksInColumnSorted[i];
                  if (currentTask.order >= taskDroppedOn.order){
                    this.changeTaskPosition(currentTask.id,currentTask.column,currentTask.order+1);
                }
              }   
          }
            if (dropWhere==="dropEnd"){
                this.changeTaskPosition(taskToDrop.id,taskDroppedOn.column,taskDroppedOn.order+1);
              }else {
                this.changeTaskPosition(taskToDrop.id,taskDroppedOn.column,taskDroppedOn.order);                  
              }
        }
        this.normalizeColumnOrder(taskDroppedOn.column);
        if (taskToDrop.column !== taskDroppedOn.column) {
          this.normalizeColumnOrder(taskToDrop.column);
        }
    },

    handleTitleClick(task){
      this.showNewTask=true;
      this.editTask=task;
    },

    handlePlusClick(){
      this.showNewTask=true;
      const randomInt = Math.floor(Math.random() * 10000);
      const tasksInColumn = [...this.tasks].filter((task)=>task.column===0);
      const TasksInOrder = tasksInColumn.length>0 ? tasksInColumn.sort((a,b)=> b.order - a.order):[{order:0}];
      const pushTask={
          id:randomInt,
          title:'New Task',
          body:'',
          column:0,
          order:TasksInOrder[0].order+1,
        };
        this.tasks.push(pushTask);
        this.editTask=this.tasks[this.tasks.length-1];
    },


    handleDelete(id){
      const taskPosition = this.tasks.findIndex((task)=>task.id===id);
      this.tasks.splice(taskPosition,1);      
    },

    handleDarkClick(){
      this.isDark = !this.isDark;
    },

    handleSwitchClick(){
      this.showBoard = !this.showBoard;
    },

  }, 
      
}
</script>

<template>
  <div :class="{dark: isDark, 'main-rotated': isPortrait, 'main': !isPortrait, 'overflow-hidden': showNewTask}">
    <div class="main-child">
      <topBar :handlePlusClick="handlePlusClick" :isDark="isDark" :handleDarkClick="handleDarkClick" :handleSwitchClick="handleSwitchClick"></topBar>
      <TaskNew v-if="showNewTask" :newTask="editTask.id" :currentTask="editTask" :isDark="isDark"  @close="showNewTask = false" @delete="handleDelete(editTask.id); showNewTask = false;"/>
      <KanbanTable v-if="showBoard" :returnTask="returnTask" :onTaskDrop="onTaskDrop" :startDrag="startDrag" :handleTitleClick="handleTitleClick" :onDropColumn="onDropColumn" :onDropBetweenTasks="onDropBetweenTasks"></KanbanTable>
      <ListView v-else :tasks="tasks" :handleTitleClick="handleTitleClick"></ListView>
      <div v-if="!isPortrait" class="bottom-flex"></div>
    </div>
  </div>
</template>
<style>

.overflow-hidden {
  overflow:hidden !important;
  overflow-block:hidden !important;
}

body {
  background-color: rgb(255, 255, 255);
  user-select: none;
}

.dark body {
  background-color: black;
}

.main {
  height: 100vh;
  width:100%;
  background-color: white;
  box-sizing: border-box;
}

.main-rotated {
    transform: rotate(-90deg)  translateX(-100%);
    transform-origin: top left;
    position:fixed;
    width: 100vh;
    height: 100vw;
    min-width: 100vh;
    min-height: 100vw;
    max-width: 100vh;
    max-height: 100vw;
    margin:0;
    background-color: white;
    box-sizing: border-box;
    overflow-block:auto;
  }

.main-child {
  width:96%;
}

.dark {
  background: rgb(88, 96, 103);
}

.margin-left {
  margin-left: 20px;
}

.bottom-flex {
    position: fixed;
    margin-top: auto; 
    bottom:0px;
    left:0px;
    width: 100%;
    height: 44px;
    background-color: rgb(0, 57, 82);
    border-top-left-radius: 80px 80px;
    border-top-right-radius: 80px 80px;
    justify-content: flex-start;
    margin-top: 8px;       
}

.dark .bottom-flex{
  color:white;
  background-color: rgb(31, 37, 39);
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

</style>