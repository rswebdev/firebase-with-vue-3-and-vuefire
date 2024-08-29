<script setup lang="ts">
import { ref } from 'vue'
import { useFirebaseAuth } from 'vuefire'
import {
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
} from '@firebase/auth'

import BaseButton from '@/components/base/BaseButton.vue'
import BaseContainer from '@/components/base/BaseContainer.vue'
import BaseCard from '@/components/base/BaseCard.vue'
import BaseForm from '@/components/base/BaseForm.vue'
import BaseInput from '@/components/base/BaseInput.vue'
import { useRouter } from 'vue-router'

const newUser = ref({
  email: '',
  password: '',
})

const authError = ref({
  code: '',
  message: '',
})

const auth = useFirebaseAuth()

const router = useRouter()

async function createUser() {
  createUserWithEmailAndPassword(
    auth!,
    newUser.value.email,
    newUser.value.password
  )
    .then((userCredential) => {
      console.log(userCredential)
      router.push('/')
    })
    .catch((error) => {
      console.log(error)
      authError.value.code = error.code
      authError.value.message = error.message
      // ..
    })
}

async function signInUser() {
  signInWithEmailAndPassword(auth!, newUser.value.email, newUser.value.password)
    .then((userCredential) => {
      const user = userCredential.user
      console.log(user)
      router.push('/')
    })
    .catch((error) => {
      authError.value.code = error.code
      authError.value.message = error.message
    })
}
</script>

<template>
  <BaseContainer>
    <h1 class="mb-4">Sign In</h1>
    <BaseCard>
      <template v-slot:default>
        <BaseForm>
          <BaseInput
            v-model="newUser.email"
            type="email"
            label="Email"
            required
            placeholder="eleanorshellstrop@thegoodplace.com"
          />
          <BaseInput
            v-model="newUser.password"
            label="Password"
            type="password"
            required
          />
        </BaseForm>
      </template>
      <template v-slot:actions>
        <BaseButton @click="signInUser" variant="tonal" color="success">
          Sign In
        </BaseButton>
        <BaseButton
          @click="createUser"
          variant="tonal"
          color="secondary"
          outline
        >
          Create New User
        </BaseButton>
      </template>
    </BaseCard>
  </BaseContainer>
</template>

<style></style>
