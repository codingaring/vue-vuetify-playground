<script setup lang="ts">
import ForComponent from '@/components/ForComponent.vue'
import TheWelcome from '@/components/TheWelcome.vue'
</script>

<template>
  <div class="about">
    <ForComponent />
    <TheWelcome />
    <RouterLink to="/user-table">User Table</RouterLink>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20rem;
    width: 100%;
  }
}
</style>
