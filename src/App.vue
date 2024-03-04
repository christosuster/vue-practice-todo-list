<script setup>
import { v4 as uuidv4 } from 'uuid'
import { ref } from 'vue'

let task

const taskList = ref([])

const handleClick = () => {
  if (!task || task == '') {
    alert('Please enter a task')
    return
  }

  taskList.value.push({ content: task, completed: false, id: uuidv4() })
  task = ''
}

const completeTask = (id) => {
  const newList = taskList.value.map((item) => {
    if (id == item.id) {
      return { ...item, completed: !item.completed }
    }
    return item
  })

  taskList.value = newList
}

const deleteTask = (id) => {
  const newList = taskList.value.filter((item) => id !== item.id)
  taskList.value = newList
}
</script>

<template>
  <div class="flex flex-col justify-between items-center gap-5 w-full h-full">
    <div class="w-full text-center flex-1">
      <h1 class="text-3xl font-bold text-white">Your Tasks</h1>
      <div class="w-full flex flex-col justify-center gap-3 mt-10 overflow-hidden">
        <div
          class="flex gap-3 justify-center items-center bg-slate-800 p-3 rounded-2xl w-full"
          v-for="item in taskList"
          :key="item.id"
          :class="{ 'bg-green-800/50': item.completed }"
        >
          <h1 class="text-left flex-1" :class="{ 'line-through text-white/60': item.completed }">
            {{ item.content }}
          </h1>
          <div class="flex justify-center gap-3 items-center">
            <button
              class="font-bold rounded-xl p-3"
              :class="{
                'bg-green-500/80 text-white': item.completed,
                'text-green-700 hover:text-green-500': !item.completed
              }"
              @click="completeTask(item.id)"
            >
              Complete
            </button>
            <button
              class="text-red-800 font-bold hover:text-red-500 rounded-xl p-3"
              @click="deleteTask(item.id)"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="w-full flex gap-3">
      <input
        class="bg-slate-700 rounded-2xl p-3 w-3/4"
        type="text"
        placeholder="Enter a task"
        v-model="task"
      />
      <button class="bg-slate-700/50 hover:bg-slate-700 rounded-2xl p-3 w-1/4" @click="handleClick">
        Add
      </button>
    </div>
  </div>
</template>
