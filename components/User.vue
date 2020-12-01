<template>
  <div>
    Name: {{ fullName }}
    <br><br>
    Message: {{ message }}
    <br><br>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="reverseMessage">
      Reverse Message
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from '@vue/composition-api'

interface User {
  firstName: string
  lastName: string
}

export default defineComponent({
  props: {
    user: {
      type: Object as () => User,
      required: true
    }
  },

  setup ({ user }) {
    const fullName = computed<string>(() => `${user.firstName} ${user.lastName}`)
    const state = reactive({
      message: 'test message !' as string
    })
    const message = computed<string>(() => state.message)
    const reverseMessage = () => {
      state.message = state.message.split('').reverse().join('')
    }

    return {
      fullName,
      message,
      reverseMessage
    }
  }
})
</script>
