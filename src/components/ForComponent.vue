<script setup lang="ts">
import { ref } from 'vue'

defineOptions({
  name: 'ForComponent',
})

const userData = ref([
  {
    name: 'John Doe',
    age: 20,
    email: 'john.doe@example.com',
    isAdmin: true,
  },
  {
    name: 'Jane Doe',
    age: 21,
    email: 'jane.doe@example.com',
    isAdmin: false,
  },
  {
    name: 'John Doe',
    age: 20,
    email: 'john.doe@example.com',
    isAdmin: true,
  },
])
</script>

<template>
  <ul>
    <li v-for="user in userData" :key="user.name">
      <span>{{ user.name }}</span>
      <span>{{ user.age }}</span>
      <span>{{ user.email }}</span>
      <span>{{ user.isAdmin }}</span>
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 10rem;
  border: 1px solid #000;
  padding: 1rem;
  margin-bottom: 1rem;
}
</style>
