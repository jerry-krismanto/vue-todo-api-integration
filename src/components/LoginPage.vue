<template>
  <h1 class="text-center m-4 text-xl font-bold">Registrasi</h1>
  <div class="flex justify-center">
    <form @submit.prevent="onSubmit" class="grid grid-cols-1 gap-4">
      <div class="grid grid-cols-2">
        <label for="name" class="m-1">name</label>
        <input v-model="name" type="text" name="name" class="border border-black rounded-md" />
      </div>

      <!-- <br /><br /> -->
      <div class="grid grid-cols-2">
        <label for="username" class="m-1">username</label>
        <input
          v-model="username"
          type="text"
          name="username"
          class="border border-black rounded-md"
        />
      </div>
      <!-- <br /><br /> -->
      <div class="grid grid-cols-2">
        <label for="password" class="m-1">password</label>
        <input
          v-model="password"
          type="text"
          name="password"
          class="border border-black rounded-md"
        />
      </div>

      <!-- <br /><br /> -->
      <input type="Submit" name="submit" class="p-1 border border-black rounded-md" />
    </form>
  </div>
  <h1 class="text-center m-4 text-xl font-bold">Login</h1>
  <div class="flex justify-center mt-8">
    <div class="grid grid-cols-1">
      <div class="grid grid-cols-1">
        <form @submit.prevent="onLogin" class="grid grid-cols-1 gap-3">
          <div class="grid grid-cols-2">
            <label for="username">Username</label>
            <input
              type="text"
              v-model="usernameLogin"
              name="username"
              class="border border-black rounded-md"
            />
          </div>
          <div class="grid grid-cols-2">
            <label for="password">Password</label>
            <input
              type="text"
              v-model="passwordLogin"
              name="password"
              class="border border-black rounded-md"
            />
          </div>
          <input
            type="Submit"
            name="submit"
            value="Login"
            class="p-1 border border-black rounded-md"
          />
        </form>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const name = ref('')
const username = ref('')
const password = ref('')
const usernameLogin = ref('')
const passwordLogin = ref('')

const emit = defineEmits(['passTokenToParent'])

const onSubmit = () => {
  handleRegister()
}

const onLogin = () => {
  handleLogin()
}

const handleRegister = async () => {
  try {
    const res = await fetch('https://be.tautan.tk/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        name: name.value,
        username: username.value,
        password: password.value
      }),
      redirect: 'follow'
    })
    if (res.ok) {
      const data = await res.json()
      console.log(data)
    }
  } catch (err) {
    console.log(err)
  }
}

const handleLogin = async () => {
  try {
    const res = await fetch('https://be.tautan.tk/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        username: usernameLogin.value,
        password: passwordLogin.value
      }),
      redirect: 'follow'
    })
    if (res.ok) {
      const data = await res.json()
      console.log(data)
      console.log(data.data.token)
      handleSession(data.data.token)
      alert('Login Success')
    }
  } catch (err) {
    console.log(err)
  }
}
const handleSession = async (token: any) => {
  try {
    const res = await fetch('https://be.tautan.tk/auth/session', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      },
      redirect: 'follow'
    })
    console.log(token)
    if (res.ok) {
      const data = await res.json()
      console.log(data)
      alert(`Hello ${data.data.name}`)
      emit('passTokenToParent', token)
    }
  } catch (err) {
    console.log(err)
  }
}
</script>
