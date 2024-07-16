<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import { useRoute } from 'vue-router'

interface Commit {
  node_id: string
  commit: {
    author: {
      date: string
    }
    message: string
  }
}

const route = useRoute()
const repoName = ref(route.params.name as string)
const commits = ref<Commit[]>([])

const fetchRepoDetails = async () => {
  const response = await fetch(
    `https://api.github.com/repos/coding-bootcamps-eu/${repoName.value}/commits`
  )
  commits.value = await response.json()
  console.log(commits.value)
}

const sortedCommits = computed(() => {
  return [...commits.value]
    .sort(
      (a, b) => new Date(b.commit.author.date).getTime() - new Date(a.commit.author.date).getTime()
    )
    .slice(0, 20)
})

onMounted(() => {
  fetchRepoDetails()
})
</script>

<template>
  <div class="liet-type3">
    <h2>{{ repoName }}</h2>
    <p>Latest 20 commits</p>
    <ul>
      <li v-for="commit in sortedCommits" :key="commit.node_id">
        {{ commit.commit.message }}
      </li>
    </ul>
  </div>
</template>
