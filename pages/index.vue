<template>
  <div class="container min-h-screen justify-center items-center text-center mx-auto p-20">
    <User :user="UserObj" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from '@vue/composition-api'
export default defineComponent({
  setup () {
    const name = ref('This is a name')
    const UserObj = ref({
      firstName: 'hey',
      lastName: 'ueo'
    })
    return {
      name,
      UserObj
    }
  }
})
</script>
