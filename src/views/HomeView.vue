<script setup lang="ts">
import LoginPage from '@/components/LoginPage.vue'
import ToDoList from '@/components/ToDoList.vue'
import { ref } from 'vue'

const tokenFromLogin = ref('')

const getToken = (token: any) => {
  tokenFromLogin.value = token
}
</script>

<template>
  <main>
    <LoginPage @passTokenToParent="getToken" />
    <br />
    <ToDoList :token="tokenFromLogin" />
  </main>
</template>
