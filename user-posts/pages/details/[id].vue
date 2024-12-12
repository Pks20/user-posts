<script setup lang="ts">
const route = useRoute();

import { ref } from "vue";

const { data, error } = await useFetch<Post>(
  `https://jsonplaceholder.typicode.com/posts/${route.params.id}`
);

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const post = ref<Post | null>(null);

if (data.value) {
  post.value = data.value;
}
</script>

<template>
  <div v-if="error" class="text-red-500">
    Failed to fetch the post detail: {{ error.message }}
  </div>

  <div
    v-else-if="post"
    class="max-w-4xl mx-auto p-6 bg-gray-800 mt-4 rounded-lg shadow-lg"
  >
    <div class="border-b pb-4 mb-6">
        <div class=" text-white mb-3">User ID: {{ post.id }}</div>
      <h2 class="text-2xl font-semibold text-white">{{ post.title }}</h2>
    </div>

    <p class="text-white whitespace-normal">{{ post.body }}</p>
  </div>

  <div v-else class="text-gray-800 text-center">Loading...</div>
</template>

<style lang="scss" scoped></style>
