<script setup lang="ts">
import { onMounted, reactive, ref, watch } from 'vue'
const title = ref('')
const description = ref('')
const category = ref('')
const todos: any = reactive([])
const token = defineProps(['token'])
// const todoData = ref()

const fetchTodos = async (token: any) => {
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
    const data = await res.json()
    console.log(data.data)
    todos.value = data.data
    console.log(todos.value)
    console.log(token)
  } catch (error) {
    console.log('an error happened')
  }
}

const addTodos = async (token: any) => {
  try {
    const res = await fetch('https://be.tautan.tk/api/todo', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      },
      body: JSON.stringify({
        title: title.value,
        description: description.value,
        category: category.value
      }),
      redirect: 'follow'
    })
    if (res.ok) {
      const data = await res.json()
      console.log(data)
      alert('Todo added')
      fetchTodos(token)
    }
  } catch (error) {
    console.log('an error happened')
  }
}

const deleteTodo = async (id: any, token: any) => {
  try {
    const res = await fetch(`https://be.tautan.tk/api/todo/${id}`, {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      },
      redirect: 'follow'
    })
    if (res.ok) {
      const data = await res.json()
      console.log(data)
      alert('Todo deleted')
      fetchTodos(token)
    }
  } catch (error) {
    console.log('an error happened')
  }
}

onMounted(() => {
  fetchTodos(token.token)
})

watch(token, () => {
  fetchTodos(token.token)
  console.log(token.token)
})
</script>

<template>
  <h1 class="text-center m-4 text-xl font-bold">Add Todo</h1>
  <div class="flex justify-center m-4">
    <form @submit.prevent="addTodos(token.token)" class="grid grid-cols-1 gap-4">
      <div class="grid grid-cols-2">
        <label for="title">Name</label>
        <input
          type="text"
          name="title"
          v-model="title"
          class="p-1 border border-black rounded-md"
        />
      </div>
      <div class="grid grid-cols-2">
        <label for="description">Description</label>
        <input
          type="text"
          name="description"
          v-model="description"
          class="p-1 border border-black rounded-md"
        />
      </div>
      <div class="grid grid-cols-2">
        <label for="category">Category</label>
        <input
          type="text"
          name="category"
          v-model="category"
          class="p-1 border border-black rounded-md"
        />
      </div>
      <input
        type="Submit"
        name="submit"
        value="Add Todo"
        class="p-1 border border-black rounded-md"
      />
    </form>
  </div>
  <div class="flex justify-center">
    <ta ble class="border-collapse">
      <thead>
        <tr class="grid grid-cols-3">
          <th class="border border-black py-2 px-4">Task</th>
          <th class="border border-black py-2 px-4">Description</th>
          <th class="border border-black py-2 px-4">Action</th>
        </tr>
      </thead>
      <tbody v-for="(todo, index) in todos.value" :key="index" :todo="todo">
        <tr class="grid grid-cols-3 gap-2">
          <td class="py-2 px-4">{{ todo.title }}</td>
          <td class="py-2 px-4">{{ todo.description }}</td>
          <td class="py-2 px-4">
            <button
              @click="deleteTodo(todo.id, token.token)"
              class="border-2 border-red-500 p-2 font-bold text-red-500"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </ta>
  </div>
</template>
