<template>
  <div class="items-center text-center mx-auto p-10">
    <h1>Uploader</h1>
    <div v-if="state.loginStatus == false">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="signIn">
        SignIn
      </button>
    </div>
    <div v-if="state.loginStatus">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="signOut">
        SignOut
      </button>
      <br><br>
      {{ state.displayName }} でログインしています
    </div>
  </div>
</template>

<script lang="ts">
import firebase from '@/plugins/firebase'
import { defineComponent, reactive, onMounted } from '@vue/composition-api'
import { Auth } from '~/types/auth'

export default defineComponent({
  setup () {
    const state = reactive<Auth>({
      displayName: '',
      loginStatus: true
    })

    const signIn = async (): Promise<void> => {
      const provider = new firebase.auth.GoogleAuthProvider()
      const credential = await firebase.auth().signInWithPopup(provider)
      const user = credential.user!
      state.displayName = user.displayName
      state.loginStatus = true
    }

    const signOut = () => {
      firebase.auth().signOut().then(function () {
        // Sign-out successful.
        state.displayName = ''
        state.loginStatus = false
      }).catch(function (error) {
        // An error happened.
        console.log(error)
      })
    }

    onMounted(() => {
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          console.log(user)
          state.displayName = user.displayName
          state.loginStatus = true
        } else {
          state.displayName = ''
          state.loginStatus = false
        }
      })
    })

    return {
      signOut, signIn, state
    }
  }
})
</script>
