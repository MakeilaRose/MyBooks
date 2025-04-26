<script setup lang="ts">
import { ref } from 'vue'
import Auth from './components/Auth.vue'
import Search from './components/Search.vue'

const signedIn = ref(false)

function handleSignedIn() {
  signedIn.value = true
}

function handleSignedOut() {
  signedIn.value = false
}
</script>

<template>
  <div class="container">
    <Auth @signed-in="handleSignedIn" @signed-out="handleSignedOut" />

    <Search v-if="signedIn" />
  </div>
</template>

<style scoped>
.container {
  text-align: center;
  padding: 20px;
}
</style>
