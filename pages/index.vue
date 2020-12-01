<template>
  <div class="container min-h-screen justify-center items-center text-center mx-auto p-20">
    <User :user="UserObj" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from '@vue/composition-api'

interface User {
  firstName: string
  lastName: string
}

export default defineComponent({
  setup () {
    const UserObj = ref<User>({
      firstName: 'ai',
      lastName: 'ueo'
    })
    return {
      UserObj
    }
  }
})
</script>
