<template>
  <div class="pt-10">
    <div class="items-center text-center mx-auto">
      <div class="bg-gray-200 py-10 bg-opacity-75">
        <h1>Advent Calender 2020 🎄</h1>
        <div v-for="(week, index) in calender" :key="index">
          <div class="flex justify-center ...">
            <div v-for="day in week" :key="day">
              <div v-if="isColor(day) == `achieved`" class="box-border h-8 w-8 p-0.5 border-2 m-1 bg-green-400 ...">
                <a :href="genLink(day)" target="_blank" rel="noopener noreferrer">{{ day }}</a>
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
        <br>
        {{ contents.length }}日連続更新中
      </div>
    </div>
    <br>
    <div class="p-5 ">
      <div v-for="content in contents" :key="content.path">
        <Link :url="`https://hukurouo.com${content.path}`" :title="content.title" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'
import { Content } from '~/types/content'

export default defineComponent({
  setup () {
    const contents: Content[] = [
      { title: 'react day16：React入門2', path: '/articles/2020-12-15-react16' },
      { title: 'react day15：React入門', path: '/articles/2020-12-14-react15' },
      { title: 'nuxt-ts day14：zennに記事書いた　そしてReactへ', path: '/articles/2020-12-13-nuxtts14' },
      { title: 'nuxt-ts day13：画像アップローダーを作ろう 3', path: '/articles/2020-12-12-nuxtts13' },
      { title: 'nuxt-ts day12：画像アップローダーを作ろう 2', path: '/articles/2020-12-11-nuxtts12' },
      { title: 'nuxt-ts day11：画像アップローダーを作ろう', path: '/articles/2020-12-10-nuxtts11' },
      { title: 'nuxt-ts day10：アドベントカレンダー製作3', path: '/articles/2020-12-09-nuxtts10' },
      { title: 'nuxt-ts day9：アドベントカレンダー製作2', path: '/articles/2020-12-08-nuxtts9' },
      { title: 'nuxt-ts day8：アドベントカレンダー製作', path: '/articles/2020-12-07-nuxtts8' },
      { title: 'nuxt-ts day7：TypeScript Deep Dive を読む 3', path: '/articles/2020-12-06-nuxtts7' },
      { title: 'nuxt-ts day6：TypeScript Deep Dive を読む 2', path: '/articles/2020-12-05-nuxtts6' },
      { title: 'nuxt-ts day5：TypeScript Deep Dive を読む', path: '/articles/2020-12-04-nuxtts5' },
      { title: 'nuxt-ts day4：axiosでランダム猫画像', path: '/articles/2020-12-03-nuxtts4' },
      { title: 'nuxt-ts day3：型安全なVuexでtodoリストを作る', path: '/articles/2020-12-02-nuxtts3' },
      { title: 'nuxt-ts day2：ESlint / 型チェックなど', path: '/articles/2020-12-01-nuxtts2' },
      { title: 'nuxt-ts day1：コンポーネントを作る', path: '/articles/2020-11-30-nuxtts1' }
    ].reverse()
    const calender : number[][] = [
      [29, 30, 1, 2, 3, 4, 5],
      [6, 7, 8, 9, 10, 11, 12],
      [13, 14, 15, 16, 17, 18, 19],
      [20, 21, 22, 23, 24, 25, 26]
    ]
    // 色のロジック変更をJSONからやる アニメーションつけてもおもろいかも
    const isColor = (date: number): string => {
      if ([29, 26].includes(date)) { return 'invalid' }
      if ([30, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14].includes(date)) { return 'achieved' }
      return 'unachieved'
    }
    const genLink = (date: number): string => {
      if (date === 30) { return 'https://hukurouo.com/articles/2020-11-30-nuxtts1' }
      const url = 'https://hukurouo.com' + contents[date].path
      return url
    }

    return {
      calender, isColor, contents, genLink
    }
  }
})
</script>
