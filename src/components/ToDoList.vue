<script setup lang="ts">
import { onMounted, onUpdated, reactive, ref } from 'vue'
const newTodo = ref('')
const todos: any = reactive({})

const fetchTodos = async () => {
  try {
    const res = await fetch('https://be.tautan.tk/api/todo', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      },
      redirect: 'follow'
    })
    // eslint-disable-next-line vue/no-mutating-props
    todos.value = await res.json()
    console.log(todos.value)
  } catch (error) {
    console.log('an error happened')
  }
}

onMounted(() => {
  fetchTodos()
})
</script>

<template>
  <div>
    <input type="text" v-model="newTodo" />
  </div>
  <div>{{}}</div>
  <table>
    <thead>
      <tr>
        <th>Task</th>
        <th>Status</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody v-for="(todo, index) in todos" :key="index" :todo="todo">
      <tr>
        <td>{{ todo.name }}</td>
        <td>{{ todo.status }}</td>
      </tr>
    </tbody>
  </table>
</template>
