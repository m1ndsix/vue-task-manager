<script setup>
import Navbar from './components/Navbar.vue'
import Jumbotron from './components/Jumbotron.vue'
import Footer from './components/Footer.vue'

import { ref, watchEffect } from 'vue'

const STORAGE_KEY = 'task-key'
const tasks = ref(JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]'))

watchEffect(() => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
})

function addTask(e){
  const value = e.target.value
  if (value) {
    tasks.value.push({
      name: value,
      completed: false
    })
  }
  e.target.value = ''
}

function deleteTask(task){
  const i = tasks.value.indexOf(task)
  if (i > -1) {
    tasks.value.splice(i, 1)
  }
}

function finishTask(task){
  const i = tasks.value.indexOf(task)
  if (i > -1) {
    tasks.value[i].completed = !tasks.value[i].completed
  }
}

</script>

<template>
  <Navbar />
  <Jumbotron />

  <div class="container mt-5 mb-2">
    <div class="row">
      <div class="col-md-4"></div>
      <div class="col-md-3">
        <div class="input-group mb-3 p-3 bg-secondary border border-info rounded">
          <input type="text" class="form-control" placeholder="Type A Task To Do Here..." @keyup.enter="addTask" aria-label="Default" aria-describedby="inputGroup-sizing-default">
        </div>
      </div>
    </div>
  </div>

  <div class="container mb-5">
    <div class="row">
      <div class="col-md-2"></div>
      <div class="col-md-5">
        <h2 class="text-head">Tasks to Do</h2>
        <div v-for="task in tasks">
          <div v-if="task.completed === false" class="text">
            {{ tasks.indexOf(task) + 1 }}
            -
            {{ task.name }}
            -
            <button type="button" class="btn btn-success" @click="finishTask(task)">✓</button>
            /
            <button type="button" class="btn btn-danger" @click="deleteTask(task)">✘</button>            
          </div>
        </div>
      </div>
      <div class="col-md-5">
        <h2 class="text-head">Finished Tasks</h2>
        <div v-for="task in tasks">
          <div v-if="task.completed === true" class="text">
            {{ tasks.indexOf(task) + 1 }}
            {{ task.name }}
            -
            <button type="button" class="btn btn-success" @click="finishTask(task)">✓</button>
            /
            <button type="button" class="btn btn-danger" @click="deleteTask(task)">✘</button>  
          </div>
        </div>
      </div>
    </div>
  </div>
  <Footer />
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Mouse+Memoirs&display=swap');

  .text {
    font-family: 'Mouse Memoirs', sans-serif;
    font-size: 30px;
  }

  .text-head {
    font-family: 'Bebas Neue', cursive;
    font-size: 50px;
  }
</style>
