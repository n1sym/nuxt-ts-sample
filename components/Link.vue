<template>
  <div class="text-purple-600 hover:text-purple-500">
    <a :href="LinkUrl" target="_blank" rel="noopener noreferrer">{{ LinkTitle }}</a>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  props: {
    url: String,
    title: String
  },

  setup ({ url, title }) {
    const LinkUrl = url
    const LinkTitle = title
    return {
      LinkUrl,
      LinkTitle
    }
  }
})
</script>
