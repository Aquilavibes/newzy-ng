<template>
  <div class="container mx-auto px-4 py-8">
    <h2 class="text-2xl font-bold mb-6">Crypto News</h2>
    <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
      <a
        v-for="article in news"
        :key="article.url"
        :href="article.url"
        target="_blank"
        rel="noopener noreferrer"
        class="block bg-white shadow rounded-2xl overflow-hidden hover:shadow-xl transition"
      >
        <img
          :src="article.urlToImage || '/default-news.jpg'"
          :alt="article.title"
          class="w-full h-48 object-cover"
        />
        <div class="p-4">
          <h3 class="text-xl font-semibold mb-2">{{ article.title }}</h3>
          <p class="text-gray-600 text-sm mb-4">
            {{ truncate(article.description || article.content || '', 100) }}
          </p>
          <div class="text-xs text-gray-400">Published: {{ timeAgo(article.publishedAt) }}</div>
        </div>
      </a>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const news = ref([])

const fetchNews = async () => {
  try {
    const response = await fetch('https://newsapi.org/v2/everything?q=crypto&apiKey=4a2c1c1b37e142d3a599d1b55099584b')
    const data = await response.json()

    if (data.status === 'ok') {
      news.value = data.articles
    } else {
      console.error('API error:', data)
    }
  } catch (error) {
    console.error('There was a problem fetching news:', error)
  }
}

const truncate = (text, length) => {
  return text.length > length ? text.slice(0, length) + '...' : text
}

const timeAgo = (dateString) => {
  const date = new Date(dateString)
  const diff = (new Date() - date) / 1000 // seconds
  if (diff < 60) return 'just now'
  if (diff < 3600) return `${Math.floor(diff / 60)} mins ago`
  if (diff < 86400) return `${Math.floor(diff / 3600)} hours ago`
  return date.toLocaleDateString()
}

onMounted(fetchNews)
</script>
