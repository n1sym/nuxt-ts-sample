<template>
  <div class="pt-10">
    <div class="items-center text-center mx-auto">
      <div class="bg-gray-200 py-10 bg-opacity-75">
        <h1>Advent Calender 2020 🎄</h1>
        <div v-for="(week, index) in calender" :key="index">
          <div class="flex justify-center ...">
            <div v-for="day in week" :key="day">
              <div v-if="isColor(day) == `achieved`" class="box-border h-8 w-8 p-0.5 border-2 m-1 bg-green-400 ...">
                {{ day }}
              </div>
              <div v-if="isColor(day) == `invalid`" class="box-border h-8 w-8 p-0.5 border-2 m-1 bg-gray-300 ...">
                {{ day }}
              </div>
              <div v-if="isColor(day) == `unachieved`" class="box-border border-gray-300 h-8 w-8 p-0.5 border-2 m-1 ...">
                {{ day }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br>
    <div class="p-5 ">
      <div v-for="content in state.obj" :key="content.path">
        <Link :url="`https://hukurouo.web.app${content.path}`" :title="content.title" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { onMounted, defineComponent, reactive } from '@vue/composition-api'
import { $axios } from '~/utils/api'

export default defineComponent({
  setup () {
    const state = reactive({
      obj: {} as object
    })
    const calender : number[][] = [
      [29, 30, 1, 2, 3, 4, 5],
      [6, 7, 8, 9, 10, 11, 12],
      [13, 14, 15, 16, 17, 18, 19],
      [20, 21, 22, 23, 24, 25, 26]
    ]
    // 色のロジック変更をJSONからやる アニメーションつけてもおもろいかも
    const isColor = (date: number): string => {
      if ([29, 26].includes(date)) { return 'invalid' }
      if ([30, 1, 2, 3, 4, 5, 6, 7, 8].includes(date)) { return 'achieved' }
      return 'unachieved'
    }
    const asyncFunc = async (): Promise<void> => {
      const JSON = await $axios.$get('https://gist.githubusercontent.com/hukurouo/58ff1826df34b20b791d0f3b49db449e/raw/c2ed76906e7448baee9555dbcb579ab8ad59f97b/content.json')
      state.obj = JSON.filter((i: { title: string|string[] }) => i.title.includes('nuxt-ts')).reverse()
    }

    onMounted(() => { asyncFunc() })
    return {
      calender, isColor, state
    }
  }
})
</script>
