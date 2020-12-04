<template>
  <div class="justify-center items-center text-center mx-auto">
    <h1>randomCat (axios)</h1>
    <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="changeCat">
      {{ state.loading }} 🐱
    </button>
    <br><br>
    <img :src="state.cat_url" alt="">
    <br><br>
    <hr>
    <br>

    <Link url="https://hukurouo.web.app/articles/2020-12-03-nuxtts4" title="nuxt-ts day4：axiosでランダム猫画像" />
  </div>
</template>

<script lang="ts">
import { onMounted, defineComponent, reactive } from '@vue/composition-api'
import { $axios } from '~/utils/api'

export default defineComponent({
  setup () {
    const state = reactive({
      cat_url: '' as string,
      loading: 'loading...' as string
    })
    const asyncFunc = async (): Promise<void> => {
      state.loading = 'loading...'
      const CatObj: {file: string} = await $axios.$get('https://aws.random.cat/meow')
      state.cat_url = CatObj.file
      state.loading = 'change !'
    }
    const changeCat = () => { asyncFunc() }

    onMounted(() => { asyncFunc() })

    return {
      state, changeCat
    }
  }
})
</script>
