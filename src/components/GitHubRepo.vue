<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'

interface Repository {
  id: number
  name: string
  commits_url: string
}

const repositories = ref<Repository[]>([])

const fetchRepos = async () => {
  const response = await fetch('https://api.github.com/users/coding-bootcamps-eu/repos')
  repositories.value = await response.json()
  console.log(repositories.value.map((e) => e.name))
}

onMounted(fetchRepos)
</script>

<template>
  <div>
    <h2>Coding Bootcamp Repositories</h2>
    <ul>
      <li v-for="repo in repositories" :key="repo.id">
        <RouterLink :to="{ name: 'DetailPage', params: { name: repo.name } }">
          {{ repo.name }}
        </RouterLink>
      </li>
    </ul>
  </div>
</template>
