<!-- components/Hero.vue -->
<template>
    <div class="relative bg-cover bg-center h-[500px] flex items-center justify-center text-white" style="background-image: url('/web3.jpg');">
      <div class="absolute inset-0 bg-black bg-opacity-60"></div>
      <div class="relative text-center px-6 max-w-3xl">
        <h1 class="text-5xl font-bold leading-tight mb-4">Stay Informed with the Latest Crypto News</h1>
        <p class="text-lg text-gray-300 mb-6">Your trusted source for breaking news, analysis, and insights from around the crypto space.</p>
        <NuxtLink to="#" class="bg-blue-500 hover:bg-blue-600 px-6 py-3 rounded-full text-white font-medium shadow-lg transition-transform transform hover:scale-105">
          Explore News
        </NuxtLink>
      </div>
    </div>
  </template>