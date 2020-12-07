<template>
  <div class="p-5">
    <div class="items-center text-center mx-auto">
      <h1>Advent Calender 2020</h1>
      <div v-for="week in calender" :key="week">
        <div class="flex justify-center ...">
          <div v-for="day in week" :key="day">
            <div v-if="isColor(day) == `achieved`" class="box-border h-8 w-8 p-0.5 border-2 m-1 bg-green-400 ...">
              {{ day }}
            </div>
            <div v-if="isColor(day) == `invalid`" class="box-border h-8 w-8 p-0.5 border-2 m-1 bg-gray-300 ...">
              {{ day }}
            </div>
            <div v-if="isColor(day) == `unachieved`" class="box-border border-gray-200 h-8 w-8 p-0.5 border-2 m-1 ...">
              {{ day }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <br>
    <br>
    <div>
      <Link url="https://hukurouo.web.app/articles/2020-11-30-nuxtts1" title="nuxt-ts day1：コンポーネントを作る" />
      <Link url="https://hukurouo.web.app/articles/2020-12-01-nuxtts2" title="nuxt-ts day2：ESlint / 型チェックなど" />
      <Link url="https://hukurouo.web.app/articles/2020-12-02-nuxtts3" title="nuxt-ts day3：型安全なVuexでtodoリストを作る" />
      <Link url="https://hukurouo.web.app/articles/2020-12-03-nuxtts4" title="nuxt-ts day4：axiosでランダム猫画像" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  setup () {
    const calender : number[][] = [
      [29, 30, 1, 2, 3, 4, 5],
      [6, 7, 8, 9, 10, 11, 12],
      [13, 14, 15, 16, 17, 18, 19],
      [20, 21, 22, 23, 24, 25, 26]
    ]
    const isColor = (date: number): string => {
      if (date === 29) { return 'invalid' }
      if ([30, 1, 2, 3, 4, 5, 6, 7].includes(date)) { return 'achieved' }
      return 'unachieved'
    }
    return {
      calender, isColor
    }
  }
})
</script>
