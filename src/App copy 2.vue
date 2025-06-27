<script setup>
import { ref} from 'vue';
      const name = ref('John Doe');
      const status = ref('active');
      const tasks = ref(['Task One', 'Task Two', 'Task Three']);
      const newTask = ref('');
      const playField = ref([0,0,0,0,0,0,0,0,0]);
      const rawHtml = "<h1>Hello h1</h1></br><p>Hallo</p>";

      const toggleStatus = () => {
        if (status.value ==='active') {
          status.value = 'pending';
        }else if (status.value === 'pending') {
          status.value = 'inactive';
        }else{
          status.value = 'active';
        }
      };
      
      const addTask = () => {
        if (newTask.value.trim() !== ''){
          tasks.value.push(newTask.value);
          newTask.value = '';
        }
      }

      const deleteTask = (index) => {
        tasks.value.splice(index,1);
      }

      const playerMove = (index)=>{
        if (playField.value[index]===0){
          playField.value[index]=1;
          ai();
        }
      }

      const ai = () =>{
        for (let i=0;i<playField.value.length;i++){          
          if (playField.value[i] === 0){
            console.log(i);
            playField.value[i]=2;
            break;
          }
        }
      }

      

</script>
<template>
  
 <h1> {{ name }}</h1>

  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pendig</p>
  <p v-else>User is inactive</p>
  <form @submit.prevent="addTask">
    <label for= "newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask"/>
    <button type="submit">Submit</button>
  </form>
  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
      {{task }}
    </span>
    <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <button v-on:click="toggleStatus">Change Status</button>
  <button @click="toggleStatus">Change Status</button>

  <table>
    <tbody>
    <button v-for="(field, index) in playField" :key="yo" @click="playerMove(index)">{{  field }}</button>
  </tbody>
  </table>
  <span v-html="rawHtml"></span>
</template>

