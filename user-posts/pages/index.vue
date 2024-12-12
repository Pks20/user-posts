<script setup lang="ts">
import { ref } from 'vue';

const {data, error}=await useFetch<Post[]>("https://jsonplaceholder.typicode.com/posts")

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const posts = ref<Post[]>([]);

if (data.value) {
  posts.value = data.value;
}

</script>

<template>
   <div v-if="error" class="text-red-500">
    Failed to fetch posts: {{ error.message }}
  </div>
  <div v-else class="container mx-auto p-4">
    <div
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8"
    >
      <div
        v-for="post in posts"
        :key="post.id"
        class="bg-gray-800 p-4 border rounded-lg shadow-md"
      >
        <NuxtLink :to="`/details/${post.id}`">
          <div class=" text-white">User ID: {{ post.id }}</div>
          <h2 class="mt-4 text-xl font-semibold text-white">{{ post.title }}</h2>
          <p class="text-white mt-2">{{ post.body }}</p>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
