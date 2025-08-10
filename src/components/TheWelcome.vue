<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)
const isZero = count.value === 0

function increment() {
  console.log('increment', count.value)
  count.value++
}

function reset() {
  count.value = 0
}
</script>

<template>
  <div class="count-box">
    <div v-if="isZero">
      <p>값을 입력해주세요.</p>
    </div>

    <button type="button" @click="increment">수량 증가</button>
    <div>수량 {{ count }}</div>
    <button type="button" @click="reset">초기화</button>
  </div>
</template>

<style scoped>
.count-box {
  width: 100%;
  display: flex;
  align-items: center;
}
</style>
