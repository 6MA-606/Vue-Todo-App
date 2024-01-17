<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const defaultData = [
  {
    done: false,
    content: 'Write a blog post',
  },
]
const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData
const todos = ref(todosData)

function addTodo() {
  if (newTodo.value) {
    todos.value.push({
      done: false,
      content: newTodo.value,
    })
    newTodo.value = ''
  }
  saveData()
}

function doneTodo(todo) {
  todo.done = !todo.done
  saveData()
}

function removeTodo(index) {
  todos.value.splice(index, 1)
  saveData()
}

function saveData() {
  const storageData = JSON.stringify(todos.value)
  localStorage.setItem('todos', storageData)
}
</script>

<template>
  <div class="w-full h-screen flex flex-col items-center">
    <div class="text-[2.5rem] font-bold m-[2.5rem]">Todo App</div>
    <form @submit.prevent="addTodo()" class="flex flex-col items-center gap-2">
      <label class="form-control w-full max-w-xs">
        <div class="label">
          <span class="label-text">New Todo</span>
        </div>
        <input
          v-model="newTodo"
          name="newTodo"
          type="text"
          autocomplete="off"
          placeholder="Type here"
          class="input input-bordered w-full max-w-xs"
        />
      </label>
      <button
        type="submit"
        class="btn btn-primary btn-sm mt-2"
        :disabled="!newTodo"
      >
        Add newTodo
      </button>
    </form>
    <div v-if="todos.length > 0" class="my-[1rem] py-[1rem]">
      <h2 class="text-xl font-bold">Todo List</h2>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <div
            class="w-96 bg-base-100 shadow-xl flex gap-3 justify-between items-center p-4 my-2 border border-base-300 rounded-lg"
          >
            <input
              @click="doneTodo(todo)"
              type="checkbox"
              class="checkbox"
              :checked="todo.done"
            />
            <div
              @click="doneTodo(todo)"
              :class="todo.done ? 'line-through opacity-50' : ''"
              class="cursor-pointer"
              >{{ todo.content }}</div
            >
            <button
              @click="removeTodo(index)"
              class="btn btn-outline btn-error btn-sm"
            >
              Remove
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped></style>
