<script>
import Button from './button.vue';
import { columnsTitleJS, columnsJS } from '@/data/columns';

export default{   
    data(){
        return {
            closeWindow:false,
            columnsTitle: columnsTitleJS,
            columns: columnsJS,
            localTask: {
                id: null,
                title: '',
                body: '',
                column: null,
                order: null
            }
        }
    },
    watch: {
        currentTask: {
            immediate: true,
            handler(newTask) {
            this.localTask = { ...newTask }; // Shallow copy
            }
        }
    },

    components: {
    Button
  },
    methods:{
        handleXClick(){
            closeWindow= true;
        }
    },
    emits:{
        close:false,
        delete:false,        
    },
    props: {
        isDark:false,
        newTask:Number,
        currentTask:{
          id: Number,
          title:String,
          body:String,
          column:Number,
          order:Number,
        }
    }
}
</script>
<template>
    <div class="task-box" @click.self="$emit('close')">
        <div class="task-display">
            <div class="top-bar">
                <h2 class="task-title" v-if = "newTask==-1">New Task</h2>
                <h2 class="task-title" v-else>Edit Task</h2>
                <Button @click="$emit('close')" class="task-close" :buttonSize="22">X</Button>
            </div>
            <div class="task-content">
                <input type="text" class="task-input task-input1" v-model="currentTask.title" />
                <textarea type="text" class="task-input task-input2"  v-model="currentTask.body" > </textarea>                
            </div>
            <div class="bottom-bar">
                <Button @click="$emit('delete')" :buttonSize="26">
                    <img v-if="!isDark" src="./../assets/bin_black.png" height="20" alt="Papierkorb"/>
                    <img v-else src="./../assets/bin.png" height="20" alt="Papierkorb"/>
                </Button>
                <div>
                    <label for="status" class="status-size">Status: </label>
                    <select class="select-styling" v-model="currentTask.column">
                        <option v-for="column in columns" :value="column">{{columnsTitle[column]}}</option>
                    </select>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
.select-styling {
    margin-top: 8px;
    font-size: large;
}

option:checked {
  background-color: rgb(0, 57, 82);
  color:white;
}
.status-size {
    font-size: large;
}
.task-title {
    font-size: x-large;
    margin-top:10px;
    margin-left: 30px;
}

.task-input {
    font-size:large;
    margin-left: 30px;
    margin-right: 30px;
}
.task-input1 {
    height: auto;
    margin-top: 10px;
    margin-bottom: 4px;
}
.task-input2 {
    margin-top: 4px;
    height: 120px;
    resize: none;
}
.task-content {
    display: flex;
    flex-direction: column;
    justify-content:flex-start;
    height: 100%;
    background-color: rgb(255, 255, 255);
}
.dark .task-content {
    background-color: rgb(0, 0, 0);
    box-shadow: 0px 0px 8px 2px rgb(183, 183, 183);
}

.dark .top-bar {
    background-color: rgb(0, 0, 0);
    box-shadow: 0px 0px 8px 2px rgb(183, 183, 183);

}
.dark .bottom-bar {
    background-color: rgb(0, 0, 0);
    box-shadow: 0px 0px 8px 2px rgb(183, 183, 183);

}
.dark textarea {
    background-color: black;
    color:white;
}
.dark input {
    background-color: black;
    color:white;
}

.top-bar {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: rgb(0, 57, 82);
    border-top-left-radius: 80px 80px;
    border-top-right-radius: 20px 20px; 
    color: white;
}
.bottom-bar {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: rgb(0, 57, 82);
    border-bottom-right-radius: 80px 80px;
    border-bottom-left-radius: 20px 20px;   
    color: white;
    padding-right: 28px;
    padding-left: 6px;
}
.bottom-h {    
    margin-left: 20px;
}
.task-display {    
    width:600px;
    height:300px;
    position: sticky;
    display: flex;
    flex-direction: column;
    background-color: rgba(255, 250, 250,0);
    border-top-left-radius: 80px 80px;
    border-top-right-radius: 20px 20px; 
    border-bottom-right-radius: 80px 80px;
    border-bottom-left-radius: 20px 20px; 
    box-shadow: 0px 0px 20px 20px rgb(38, 38, 40);
}
.task-box{    
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.4);    
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}
</style>