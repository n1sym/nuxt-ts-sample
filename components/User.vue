<template>
  <div>
    Name: {{ fullName }}
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from '@vue/composition-api'
import { User } from '~/types/user'

export default defineComponent({
  props: {
    user: {
      type: Object as () => User,
      required: true
    }
  },

  setup ({ user }) {
    const fullName = computed<string>(() => `${user.firstName} ${user.lastName}`)
    return {
      fullName
    }
  }
})
</script>
