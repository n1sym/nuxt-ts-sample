<template>
  <div>
    <p class="py-5">
      作ったもの置き場
      <br>
      <Link url="https://github.com/hukurouo/nuxt-ts-sample" title="https://github.com/hukurouo/nuxt-ts-sample" />
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
      { link: 'user', title: '基本的なcomponent' },
      { link: 'reverse_text', title: 'テキスト反転' },
      { link: 'todolist', title: 'TODOリスト (Vuex)' },
      { link: 'axios', title: 'randomCat (axios)' }
    ]
    return {
      items
    }
  }
})
</script>
