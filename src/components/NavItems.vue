<script setup lang="ts">
import { signOut } from 'firebase/auth'
import { useCurrentUser, useFirebaseAuth } from 'vuefire'
import BaseButton from '@/components/base/BaseButton.vue'
import { useRouter } from 'vue-router'

const auth = useFirebaseAuth()
const user = useCurrentUser()
const router = useRouter()

async function signOff() {
  signOut(auth!)
    .then(() => {
      router.push('/sign-in')
    })
    .catch((error) => {
      console.log('An error happened.', error)
    })
}
</script>

<template>
  <nav class="pr-4">
    <BaseButton to="/">Home</BaseButton>
    <BaseButton to="/new">New</BaseButton>
    <BaseButton @click="signOff" v-if="user?.email">Sign-Out</BaseButton>
    <BaseButton to="/sign-in" v-else>Sign-In</BaseButton>
  </nav>
</template>

<style></style>
