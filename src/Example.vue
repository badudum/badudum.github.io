
  
<script>
import { ref, onMounted } from 'vue';

// COMPOSITION API
export default{
    setup(){
        const name = ref('David Lee');
        const status = ref('active');
        const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
        const link = ref("https://www.google.com");
        const newTask = ref('');

        const toggleStatus = () => {
            status.value = status.value === 'active' ? 'pending' : 'active';
        }
        const addTask = () => {
            if (newTask.value.trim() !== ''){
                tasks.value.push(newTask.value);
                newTask.value = '';
            }
        }

        const deleteTask = (index) => {
            tasks.value.splice(index, 1);
        }

        onMounted(async () => {
            try {
              const response = await fetch('https://jsonplaceholder.typicode.com/todos');
              const data = await response.json();
              tasks.value = data.map((task) => task.title);
            }catch (error) {
              console.error("error fetching data", error);
            }
        });


        return { name, status, tasks, link, newTask, toggleStatus, addTask, deleteTask};
    },
};



// OPTIONS API
// export default {
//   data() {
//     return {
//       name : 'David Lee',
//       status: 'active',
//       tasks: ['Task 1', 'Task 2', 'Task 3'],
//       link : "https://www.google.com"
//     };
//   },
//   methods : {
//     toggleStatus() {
//       this.status = this.status === 'active' ? 'pending' : 'active';
//     }
//   }
// };
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'"> hello</p>
  <p v-else-if="status === 'pending'"> ummm </p>
  <p v-else>bye</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name ="newTask" v-model="newTask" >
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="(task,index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">Delete</button>
    </li> 
  </ul>

  <a :href="link">Google</a>

  <button v-on:click="toggleStatus">Change status</button>
</template>

<style scoped>

</style>
