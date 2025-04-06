<template>
    <div class="container mx-auto px-4 py-8">
      <h2 class="text-2xl font-bold mb-6">Latest News</h2>
      <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
        <div v-for="newsItem in news" :key="newsItem.id" class="bg-white shadow rounded-2xl overflow-hidden hover:shadow-xl transition">
          <a v-for="newsItem in news"
        :key="newsItem.id"
        :href="newsItem.url"
        target="_blank"
        rel="noopener noreferrer"
        class="block bg-white shadow rounded-2xl overflow-hidden hover:shadow-xl transition"
      >
          <img :src="newsItem.image" alt="newsItem.title" class="w-full h-48 object-cover">
          <div class="p-4">
            <h3 class="text-xl font-semibold mb-2">{{ newsItem.title }}</h3>
            <p class="text-gray-600 text-sm mb-4">{{ truncate(newsItem.text, 100) }}</p>
            <div class="text-xs text-gray-400">Posted {{ timeAgo(newsItem.publish_date) }}</div>
          </div>
          </a>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const news = ref([]);
  
  // Fetch news using the provided API
  const fetchNews = async () => {
    const url = 'https://api.worldnewsapi.com/top-news?source-country=ng&language=en&date=2025-04-05';
    const apiKey = 'ab411aa672d14681897f18325b82456f';
  
    try {
      const response = await fetch(url, {
        method: 'GET',
        headers: {
          'x-api-key': apiKey
        }
      });
  
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
  
      const data = await response.json();
      
      // Log the response to check its structure
      console.log(data); 
  
      // Check if 'top_news' exists and contains news data
      if (data && data.top_news && data.top_news.length > 0) {
        // Extract news items from the 'top_news' array
        const allNews = data.top_news.flatMap((category) => category.news);
  
        // Update the news array
        news.value = allNews.map((newsItem) => ({
          id: newsItem.id,
          title: newsItem.title,
          text: newsItem.text || 'No text available.',
          publish_date: newsItem.publish_date,
          image: newsItem.image || '/default-news.jpg',
          url: newsItem.url
        }));
      } else {
        console.error('No news found or top_news is empty.');
        news.value = []; // Fallback to an empty array if no news is found
      }
  
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  };
  
  onMounted(fetchNews);
  
  // Function to truncate text
  function truncate(text, maxLength) {
    return text.length > maxLength ? text.slice(0, maxLength) + '...' : text;
  }
  
  // Function to calculate how long ago the news was posted
  function timeAgo(dateStr) {
    const diff = (Date.now() - new Date(dateStr)) / 1000;
    const hours = Math.floor(diff / 3600);
    const days = Math.floor(diff / 86400);
    if (days > 0) return `${days} day${days > 1 ? 's' : ''} ago`;
    if (hours > 0) return `${hours} hour${hours > 1 ? 's' : ''} ago`;
    return 'Just now';
  }
  </script>
  