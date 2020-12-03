<template>
  <div class="items-center text-center mx-auto">
    <h1>基本的なcomponent</h1>
    <User :user="UserObj" />
    <br>
    <Link url="https://hukurouo.web.app/articles/2020-11-30-nuxtts1" title="nuxt-ts day1：コンポーネントを作る" />
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
      firstName: 'tanaka',
      lastName: 'tarou'
    })
    return {
      UserObj
    }
  }
})
</script>
