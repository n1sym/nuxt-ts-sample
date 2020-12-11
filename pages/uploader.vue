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
      {{ state.displayName }} でログインしています {{ state.authStatus }}
    </div>
  </div>
</template>

<script lang="ts">
import firebase from '@/plugins/firebase'
import { defineComponent, reactive, onMounted } from '@vue/composition-api'
import { Auth } from '~/types/auth'

const db = firebase.firestore()
const docRef = db.collection('datas').doc('dDcpHNalFtXE6VOrBip6')

export default defineComponent({
  setup () {
    const state = reactive<Auth>({
      displayName: '',
      loginStatus: false,
      authStatus: false
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
        // eslint-disable-next-line no-console
        console.log(error)
        // An error happened.
      })
    }

    onMounted(async (): Promise<void> => {
      await firebase.auth().onAuthStateChanged(async (user) => {
        let email: string | undefined = ''
        await docRef.get().then(function (doc) {
          if (doc.exists) {
            email = doc.data()!.email
          } else {
          // doc.data() will be undefined in this case
            console.log('No such document!')
          }
        }).catch(function (error) {
          console.log('Error getting document:', error)
        })
        if (user) {
          state.displayName = user.displayName
          state.loginStatus = true
          if (email === user.email) {
            state.authStatus = true
          }
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
