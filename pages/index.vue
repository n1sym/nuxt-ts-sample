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
      { link: 'reverse_text', title: 'テキスト反転' },
      { link: 'user', title: '基本的なcomponent' },
      { link: 'todolist', title: 'TODOリスト(Vuex)' }
    ]
    return {
      items
    }
  }
})
</script>
