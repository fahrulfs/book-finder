<script setup>
import { ref, defineEmits } from "vue"

const keyword = ref("");
const emits = defineEmits(['search'])
</script>

<template>
  <form class="mt-5 flex justify-center gap-3">
    <input
      type="search"
      v-model="keyword"
      class="w-[60vw] rounded-md focus:outline-none focus-within:ring-zinc-900 focus:border-zinc-900"
      placeholder="Type book name,author..."
    />
    <button
      @click.prevent="emits('search', { keyword })"
      class="py-2 px-4 rounded-md bg-yellow-600 text-zinc-100 hover:bg-yellow-600/90"
    >
      Search
    </button>

  </form>
   <section class="mt-5">
      <h2 v-if="keyword" class="text-white">Result for "<span class="text-yellow-500">{{ keyword }}</span>"</h2>
      </section>
</template>