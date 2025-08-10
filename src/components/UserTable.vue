<script setup lang="ts">
import { computed, ref, watchEffect } from 'vue'
import type { DataTableHeader } from 'vuetify'

type User = {
  id: number
  name: string
  email: string
  role: 'admin' | 'user'
  active: boolean
}

const headers: DataTableHeader[] = [
  { title: 'ID', key: 'id', width: 80 },
  { title: '이름', key: 'name' },
  { title: '이메일', key: 'email' },
  { title: '역할', key: 'role' },
  { title: '상태', key: 'active' },
  { title: '액션', key: 'actions', sortable: false, align: 'end', width: 140 },
]

const raw = ref<User[]>([
  { id: 1, name: 'Alice', email: 'alice@example.com', role: 'admin', active: true },
  { id: 2, name: 'Bob', email: 'bob@example.com', role: 'user', active: false },
  { id: 3, name: 'Cara', email: 'cara@example.com', role: 'user', active: true },
  { id: 4, name: 'Dave', email: 'dave@example.com', role: 'admin', active: true },
  { id: 5, name: 'Eve', email: 'eve@example.com', role: 'user', active: false },
  { id: 6, name: 'Frank', email: 'frank@example.com', role: 'admin', active: true },
  { id: 7, name: 'Grace', email: 'grace@example.com', role: 'user', active: false },
  { id: 8, name: 'Hank', email: 'hank@example.com', role: 'admin', active: true },
  { id: 9, name: 'Ivy', email: 'ivy@example.com', role: 'user', active: false },
  { id: 10, name: 'Jack', email: 'jack@example.com', role: 'admin', active: true },
  { id: 11, name: 'Kate', email: 'kate@example.com', role: 'user', active: false },
  { id: 12, name: 'Liam', email: 'liam@example.com', role: 'admin', active: true },
  { id: 13, name: 'Mia', email: 'mia@example.com', role: 'user', active: false },
  { id: 14, name: 'Noah', email: 'noah@example.com', role: 'admin', active: true },
  { id: 15, name: 'Olivia', email: 'olivia@example.com', role: 'user', active: false },
  { id: 16, name: 'Parker', email: 'parker@example.com', role: 'admin', active: true },
  { id: 17, name: 'Quinn', email: 'quinn@example.com', role: 'user', active: false },
  { id: 18, name: 'Ryan', email: 'ryan@example.com', role: 'admin', active: true },
  { id: 19, name: 'Sophia', email: 'sophia@example.com', role: 'user', active: false },
  { id: 20, name: 'Thomas', email: 'thomas@example.com', role: 'admin', active: true },
  { id: 21, name: 'Uma', email: 'uma@example.com', role: 'user', active: false },
  { id: 22, name: 'Vincent', email: 'vincent@example.com', role: 'admin', active: true },
  { id: 23, name: 'Wendy', email: 'wendy@example.com', role: 'user', active: false },
  { id: 24, name: 'Xavier', email: 'xavier@example.com', role: 'admin', active: true },
  { id: 25, name: 'Yara', email: 'yara@example.com', role: 'user', active: false },
  { id: 26, name: 'Zane', email: 'zane@example.com', role: 'admin', active: true },
  { id: 27, name: 'Ava', email: 'ava@example.com', role: 'user', active: false },
  { id: 28, name: 'Ben', email: 'ben@example.com', role: 'admin', active: true },
  { id: 29, name: 'Cora', email: 'cora@example.com', role: 'user', active: false },
  { id: 30, name: 'Dylan', email: 'dylan@example.com', role: 'admin', active: true },
])

// 검색어, 역할, 페이지, 페이지당 아이템 수
const q = ref('')
const role = ref<'all' | 'admin' | 'user'>('all')
const page = ref(1)
const itemsPerPage = ref(10)

// 검색어 + 역할 필터링
const filteredItems = computed(() => {
  const keyword = q.value.trim().toLowerCase()
  const roleFilter = role.value

  return raw.value.filter((u) => {
    const hitRole = roleFilter === 'all' ? true : u.role === roleFilter
    const hay = (u.name + ' ' + u.email).toLowerCase()
    const hitKeyword = keyword === '' ? true : hay.includes(keyword)
    return hitRole && hitKeyword
  })
})

// 페이지 계산
const pageCount = computed(() =>
  Math.max(1, Math.ceil(filteredItems.value.length / itemsPerPage.value)),
)

watchEffect(() => {
  if (page.value > pageCount.value) page.value = 1
})

// 현재 페이지 아이템 계산
const pagedItems = computed(() => {
  const start = (page.value - 1) * itemsPerPage.value
  return filteredItems.value.slice(start, start + itemsPerPage.value)
})

function editUser(u: User) {
  console.log('edit', u)
}
function toggleActive(u: User) {
  u.active = !u.active
}
</script>

<template>
  <div class="d-flex flex-wrap gap-4 mb-4">
    <v-text-field
      v-model="q"
      label="검색 (이름/이메일)"
      prepend-inner-icon="mdi-magnify"
      density="compact"
      hide-details
      style="max-width: 320px"
    />
    <v-select
      v-model="role"
      :items="[
        { title: '전체', value: 'all' },
        { title: '관리자', value: 'admin' },
        { title: '사용자', value: 'user' },
      ]"
      label="역할"
      density="compact"
      hide-details
      style="width: 160px"
    />
    <v-select
      v-model="itemsPerPage"
      :items="[5, 10, 20, 50]"
      label="페이지당"
      density="compact"
      hide-details
      style="width: 140px"
    />
    <div class="ms-auto d-flex align-center text-medium-emphasis">
      총 {{ filteredItems.length }}건
    </div>
  </div>

  <v-data-table :headers="headers" :items="pagedItems" item-key="id" class="mb-2">
    <template #[`item.role`]="{ item }">
      <v-chip :color="item.role === 'admin' ? 'primary' : 'default'" size="small" label>
        {{ item.role }}
      </v-chip>
    </template>

    <template #[`item.active`]="{ item }">
      <v-chip :color="item.active ? 'success' : 'warning'" size="small" label>
        {{ item.active ? '활성' : '중지' }}
      </v-chip>
    </template>

    <template #[`item.actions`]="{ item }">
      <v-btn size="small" variant="text" @click="editUser(item)">수정</v-btn>
      <v-btn size="small" variant="text" @click="toggleActive(item)">
        {{ item.active ? '중지' : '활성' }}
      </v-btn>
    </template>
  </v-data-table>

  <!-- 페이지네이션 컨트롤 -->
  <div class="d-flex justify-end">
    <v-pagination v-model="page" :length="pageCount" total-visible="7" density="comfortable" />
  </div>
</template>
