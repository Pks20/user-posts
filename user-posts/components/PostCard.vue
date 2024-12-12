<script setup lang="ts">

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

defineProps<{
  post: Post;
}>();
</script>

<template>
  <div class="bg-gray-800 p-4 border rounded-lg shadow-md">
    <NuxtLink :to="`/details/${post.id}`">
      <div class="text-white">User ID: {{ post.id }}</div>
      <h2 class="mt-4 text-xl font-semibold text-white">
        {{ post.title }}
      </h2>
      <p class="text-white mt-2 w-full truncate">{{ post.body }}</p>
    </NuxtLink>
  </div>
</template>
