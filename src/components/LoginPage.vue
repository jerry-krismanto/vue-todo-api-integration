<template>
  <form @submit.prevent="onSubmit">
    <label for="name">name</label>
    <input type="text" name="name" />
    <br /><br />
    <label for="username">username</label>
    <input type="text" name="username" />
    <br /><br />
    <label for="password">password</label>
    <input type="text" name="password" />
    <br /><br />
    <input type="Submit" name="submit" />
  </form>
  <button @click="handleLogin">Login</button>
  <br />
  <button @click="handleSession">Session Check</button>
</template>

<script setup lang="ts">

const onSubmit = () => {
  handleRegister()
}

const handleRegister = async () => {
  try {
    const res = await fetch('https://be.tautan.tk/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        name: 'test',
        username: 'test',
        password: 'test'
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
        username: 'test',
        password: 'test'
      }),
      redirect: 'follow'
    })
    if (res.ok) {
      const data = await res.json()
      console.log(data)
      console.log(data.data.token)
      handleSession(data.data.token)
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
    }
  } catch (err) {
    console.log(err)
  }
}
</script>
