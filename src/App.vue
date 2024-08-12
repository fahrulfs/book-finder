<script setup>
import FormSearch from './components/FormSearch.vue'
import { ref } from 'vue'
import Card from './components/Card.vue'

const books = ref({ items: [], totalItems: 0 })
const keyword = ref('')
async function search({ keyword }) {
  if (!keyword.trim()) {
    books.value = { items: [], totalItems: 0 };
    return;
  }
  const url = new URL(import.meta.env.VITE_BASE_URL)
  url.searchParams.set("q", keyword)
  try {
    const res = await fetch(url)
    const data = await res.json()
    books.value = {
      items: data.items || [], 
      totalItems: data.totalItems || 0 
    }
  } catch (error) {
    console.error("Failed to fetch books:", error)
    books.value = { items: [], totalItems: 0 }
  }
}
</script>
<template>
  <div :class="{'h-full': books.items.length > 0, 'h-dvh': books.items.length === 0}" class="relative w-full py-5 flex justify-center items-center">
    <section class="container text-center z-20 flex flex-col items-center justify-center">
      <a href="/" class="cursor-pointer">
        <h1 class="text-yellow-500">The Book Finder</h1>
      </a>
      <p class="text-white">Hi, I'm The Bookfinder and I'm going to help you to find that book you were searching for,
        using the Google Books API! Enjoy it!</p>
      <FormSearch @search="search" />
      <div class="w-full overflow-y-auto h-fit grid grid-cols-1 lg:grid-cols-2 gap-3 text-left text-white">
        <Card class="block text-left" v-for="{ volumeInfo, id } of books.items" :key="id">
          <template #header>
            <figure>
              <img :src="volumeInfo?.imageLinks?.thumbnail" alt="Book Cover" class="w-32 h-48">
            </figure>
          </template>
          <template #content>
            <h4>{{ volumeInfo?.title }}</h4>
            <p class="italic">Publisher : {{ volumeInfo?.publisher }}</p>
            <p class="text-sm text-gray-400">Published In : {{ volumeInfo?.publishedDate }}</p>
            <a class="pt-2" :href="volumeInfo?.infoLink" target="_blank">
              <button class="mt-3 text-black bg-yellow-500 px-3 py-1 rounded-md">More Info</button>
            </a>
          </template>
        </Card>
      </div>
    </section>
    <div class="absolute inset-0 bg-black/50"></div>
  </div>
</template>