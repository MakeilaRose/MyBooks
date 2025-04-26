<script setup lang="ts">
import { ref, onMounted, defineEmits } from 'vue'
import { gapi } from 'gapi-script'

const isSignedIn = ref(false)
const userProfile = ref<any>(null)

const emit = defineEmits(['signed-in', 'signed-out'])

function initClient() {
  gapi.load('client:auth2', () => {
    gapi.client.init({
      apiKey: 'AIzaSyBjfnWVtRY5POPgp5GSM2wOMZxDMCSwl2A',
      clientId: '672791505215-5hjli653lmos202u6on5gqkpcrhr6meh.apps.googleusercontent.com',
      discoveryDocs: ['https://www.googleapis.com/discovery/v1/apis/books/v1/rest'],
      scope: 'https://www.googleapis.com/auth/books',
    }).then(() => {
      const authInstance = gapi.auth2.getAuthInstance()
      authInstance.isSignedIn.listen(updateSigninStatus)
      updateSigninStatus(authInstance.isSignedIn.get())
    })
  })
}

function updateSigninStatus(signedIn: boolean) {
  isSignedIn.value = signedIn
  if (signedIn) {
    userProfile.value = gapi.auth2.getAuthInstance().currentUser.get().getBasicProfile()
    emit('signed-in', userProfile.value)
  } else {
    userProfile.value = null
    emit('signed-out')
  }
}

function handleSignInClick() {
  gapi.auth2.getAuthInstance().signIn()
}

function handleSignOutClick() {
  gapi.auth2.getAuthInstance().signOut()
}

onMounted(() => {
  initClient()
})
</script>

<template>
  <div>
    <button v-if="!isSignedIn" @click="handleSignInClick">Sign in with Google</button>
    <div v-else>
      <p>Welcome, {{ userProfile?.getName() }}</p>
      <button @click="handleSignOutClick">Sign out</button>
    </div>
  </div>
</template>

<style scoped>
button {
  padding: 8px 12px;
  margin: 10px;
  cursor: pointer;
}
</style>
