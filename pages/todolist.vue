<template>
  <div class="justify-center items-center text-center max-w-md mx-auto">
    <h1>Todoリスト</h1>
    <div class="flex">
      <input v-model="state.todo" class="bg-white focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block w-full leading-normal" placeholder="taskを入力してください">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-20 ml-3" @click="addTodo">
        追加
      </button>
    </div>
    <br>
    <li v-for="(todo, index) in todolist" :key="index" class="py-1 text-left">
      {{ todo }}
      <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 ml-3 rounded-full" @click="removeTodo(index)">
        delete
      </button>
    </li>
    <br><br>
    <hr>
    <br>

    <Link url="https://hukurouo.web.app/articles/2020-12-02-nuxtts3" title="nuxt-ts day3：型安全なVuexでtodoリストを作る" />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from '@vue/composition-api'
import { TodoStore } from '~/store'
import { Todo } from '~/types/todo'

export default defineComponent({
  setup () {
    const state = reactive<Todo>({
      todo: ''
    })
    const todos = TodoStore
    const todolist = computed(() => todos.getTodos)

    const addTodo = () => {
      todos.add(state.todo)
      state.todo = ''
    }
    const removeTodo = (index: number) => {
      todos.remove(index)
    }

    return {
      state,
      todolist,
      addTodo,
      removeTodo
    }
  }
})
</script>
