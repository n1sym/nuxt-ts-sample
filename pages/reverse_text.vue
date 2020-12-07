<template>
  <div class="justify-center items-center text-center mx-auto p-10">
    <h1>テキスト反転</h1>
    Message: {{ message }}
    <br><br>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="reverseMessage">
      Reverse Message
    </button>
    <br><br>
    <Link url="https://hukurouo.web.app/articles/2020-12-01-nuxtts2" title="nuxt-ts day2：ESlint / 型チェックなど" />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from '@vue/composition-api'

export default defineComponent({
  setup () {
    const state = reactive({
      message: 'test message !' as string
    })
    const message = computed<string>(() => state.message)
    const reverseMessage = () => {
      state.message = state.message.split('').reverse().join('')
    }

    return {
      message,
      reverseMessage
    }
  }
})
</script>
