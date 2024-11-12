<template>
  <header class="bg-green-600 p-2">
    <div class="container">
      <h4 class="text-center text-white">TO DO</h4>
    </div>
  </header>

  <main>
    <section>
      <div class="container">

        <form action="#" @submit.prevent="addTask" class="mx-auto w-[400px] bg-slate-400 p-5 rounded-2xl mt-12">
          <label for="task-title" class="mb-4 block">
            <p>Enter task title</p>
            <input v-model="taskTitle" class="h-8 w-full mt-1 rounded-xl px-4" type="text" placeholder="Enter task title" id="task-title">
          </label>
          <label for="task-deadline" class="mb-4 block">
            <p>Enter task deadline</p>
            <input v-model="taskDeadline" class="h-8 w-full mt-1 rounded-xl px-4" type="date" placeholder="Enter task deadline" id="task-deadline">
          </label>

          <button type="submit" class="bg-green-700 rounded-3xl focus:bg-green-600 text-white px-3 py-2">Add task</button>
        </form>

        <div v-if="!tasks.length" class="mx-auto bg-slate-300 w-[400px] rounded-xl my-4 text-center">
          <p>Not found tasks!</p>
        </div>

        <ul v-else class="mx-auto w-[400px] rounded-2xl p-2 my-4 bg-slate-500 ">
          <li v-for="(el, index) in tasks" class="relative bg-white p-3 mb-2 rounded-xl">
            <strong class="absolute top-0 text-xs">#{{ index+1 }}</strong>
            <p v-if="el.complated" class="mt-2 block text-xs line-through">{{ el.title }}</p>
            <p v-else class="mt-2 block text-xs">{{ el.title }}</p>
            <button @click="()=>deleteTask(el.id)" class="bg-red-600 focus:bg-red-500 me-1 text-white px-4 rounded-xl">X</button>
            <button @click="completedTask(el.id)" :disabled="el.complated" class="bg-green-700 disabled:bg-green-100 focus:bg-green-500 text-white px-4 rounded-xl">done</button>
          </li>
        </ul>

      </div>
    </section>
  </main>

  <footer class="bg-green-600 text-white text-center p-4">
    <p>{{ new Date().getFullYear() }}</p>
  </footer>

</template>

<script setup>

import { ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const tasks = ref([]);

const taskTitle = ref("");
const taskDeadline = ref("");


const addTask=()=>{
  if(taskDeadline.value.length && taskTitle.value.trim().length){
      const newTask={
        id:uuidv4(),
        created_at: Date.now(),
        title:taskTitle.value,
        deadline:taskDeadline.value,
        complated:false,
      }

      tasks.value.push(newTask)
      taskTitle.value="";
      taskDeadline.value="";
      toast.success("Task added successfully!", {
        autoClose:1000
      });
  }else{
    alert("Please add task title and deadline ");
    toast.error("Task title and deadline are required!", {
      autoClose:1000
    })
  }
  
}

const deleteTask=(id)=>{
  tasks.value=tasks.value.filter((item)=>item.id !=id);
  toast.success("Task deleted successfully!", {
    autoClose:1000
  })
}

const completedTask=(id)=>{
  tasks.value.forEach((item)=>{
    if(item.id===id){
      item.complated= true;
    }
  });
  toast.success("Task completed successfully!", {
    autoClose:1000
  })
}

</script>

<style lang="scss" scoped></style>
