<template>
  <div>
    <p class="py-5">
      作ったもの置き場
      <br>
      <a class="text-purple-600 hover:text-purple-300" href="https://github.com/hukurouo/nuxt-ts-sample">https://github.com/hukurouo/nuxt-ts-sample</a>
    </p>
    <li v-for="item in items" :key="item.key" class="py-1">
      <nuxt-link :to="`/${item.link}`" class="hover:underline">
        {{ item.title }}
      </nuxt-link>
    </li>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  setup () {
    const items = [
      { link: 'reverse_text', title: 'テキスト反転' },
      { link: 'user', title: '基本的なcomponent' },
      { link: 'todolist', title: 'TODOリスト' }
    ]
    return {
      items
    }
  }
})
</script>
