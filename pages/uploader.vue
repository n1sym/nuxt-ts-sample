<template>
  <div class="items-center text-center mx-auto p-5">
    <h1>Uploader</h1>
    <div v-if="state.authStatus">
      <div class="mt-2 flex justify-center px-6 pt-5 pb-6 border-2 border-gray-300 border-dashed rounded-md">
        <div class="space-y-1 text-center">
          <svg class="mx-auto h-12 w-12 text-gray-400" stroke="currentColor" fill="none" viewBox="0 0 48 48" aria-hidden="true">
            <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
          </svg>
          <div class="flex justify-center text-sm text-gray-600">
            <label for="file-upload" class="relative cursor-pointer bg-white rounded-md font-medium text-indigo-600 hover:text-indigo-500 focus-within:outline-none focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-indigo-500">
              <span>Upload a file</span>
              <input id="file-upload" name="file-upload" type="file" class="sr-only" @change="uplaodFile">
            </label>
          </div>
          <p class="text-xs text-gray-500">
            PNG, JPG, GIF up to 10MB
          </p>
        </div>
      </div>
      <br>
      <div v-if="info.message != ''" class="p-2 sm:w w-full">
        <div class="bg-green-200 rounded flex p-4 h-full items-center">
          <svg
            fill="none"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="3"
            class="text-green-500 w-6 h-6 flex-shrink-0 mr-4"
            viewBox="0 0 24 24"
          >
            <path d="M22 11.08V12a10 10 0 11-5.93-9.14" />
            <path d="M22 4L12 14.01l-3-3" />
          </svg>
          <span class="title-font font-medium antialiased ">{{ info.message }}</span>
        </div>
      </div>
      <br>
      <div class="flex flex-wrap justify-center ...">
        <div v-for="item in info.imagelist" :key="item.key" class="py-1 text-gray-800 break-all">
          <button class="hover:bg-blue-700 text-white font-bold py-1 px-1 rounded" @click="copySomething(item)">
            <img :src="item" alt="" class="w-24">
          </button>
        </div>
      </div>
      <br><br>
      ✅ 認証済み
      <br><br>
    </div>
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
    </div>
  </div>
</template>

<script lang="ts">
import firebase from '@/plugins/firebase'
import { defineComponent, reactive, onMounted } from '@vue/composition-api'
import { Auth } from '~/types/auth'

const db = firebase.firestore()
const docRef = db.collection('datas').doc('dDcpHNalFtXE6VOrBip6')
const storageRef = firebase.storage().ref()

export default defineComponent({
  setup () {
    const state = reactive<Auth>({
      displayName: '',
      loginStatus: false,
      authStatus: false
    })

    const info = reactive({
      message: '' as string,
      imagelist: [] as string[]
    })

    const signIn = async (): Promise<void> => {
      const provider = new firebase.auth.GoogleAuthProvider()
      const credential = await firebase.auth().signInWithPopup(provider)
      const user = credential.user
      if (user) {
        state.displayName = user.displayName
        state.loginStatus = true
        isAuth(user.email)
      }
    }

    const signOut = (): void => {
      firebase.auth().signOut().then(function () {
        state.displayName = ''
        state.loginStatus = false
        state.authStatus = false
      }).catch(function (error) {
        console.log(error)
      })
    }

    const isAuth = (email: string | null): void => {
      docRef.get().then(function (doc) {
        if (doc.exists) {
          if (email === doc.data()!.email) {
            state.authStatus = true
            listImageFile()
          }
        } else {
        // doc.data() will be undefined in this case
          console.log('No such document!')
        }
      }).catch(function (error) {
        console.log('Error getting document:', error)
      })
    }

    const uplaodFile = (event: any): void => {
      info.message = ''
      const file = event.target.files[0]
      if (!file) { return }
      if (file.size >= 10000000) { return }
      const Ref = storageRef.child('images/' + file.name)
      Ref.put(file).then(() => {
        firebase.storage().ref('images/' + file.name).getDownloadURL().then((url) => {
          db.collection('images').add({
            name: file.name,
            image_url: url,
            size: file.size,
            time: new Date().toLocaleString('ja-JP', { timeZone: 'Asia/Tokyo' })
          })
            .then(function (docRef) {
              info.message = 'アップロードが完了しました'
              listImageFile()
              console.log('Document written with ID: ', docRef.id)
            })
            .catch(function (error) {
              console.error('Error adding document: ', error)
            })
        }).catch((error) => {
          console.log(error)
        })
      })
      setTimeout(function () { info.message = '' }, 5000)
    }

    const listImageFile = (): void => {
      db.collection('images')
        .orderBy('time', 'desc').limit(12)
        .get()
        .then((querySnapshot) => {
          info.imagelist = []
          querySnapshot.forEach((doc) => {
            info.imagelist.push(doc.data().image_url)
          })
        })
        .catch(function (error) {
          console.log('Error getting documents: ', error)
        })
    }

    const copySomething = (text: string): void => {
      navigator.clipboard.writeText('<figure><img src="' + (text) + '"><figcaption></figcaption></figure>').catch((e) => {
        console.error(e)
      })
    }

    onMounted(() => {
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          state.displayName = user.displayName
          state.loginStatus = true
          isAuth(user.email)
        } else {
          state.displayName = ''
          state.loginStatus = false
        }
      })
    })

    return {
      signOut, signIn, state, uplaodFile, info, copySomething
    }
  }
})
</script>
