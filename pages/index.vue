<template>
  <div class="container min-h-screen justify-center items-center text-center mx-auto p-20">
    <User :user="user_obj"/>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from '@vue/composition-api'
export default defineComponent({
  setup () {
    const name = ref('This is a name')
    const user_obj = ref({firstName: "ai", 
                          lastName:  "ueo"})
    return {
      name,
      user_obj
    }
  }
})
</script>