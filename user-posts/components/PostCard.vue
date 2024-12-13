<script setup lang="ts">
import { type Post } from "../types/Post";

defineProps<{
  post: Post;
}>();

function handleEdit() {
  console.log("Edit post");
}

function handleDelete() {
  console.log("Delete post");
}
</script>

<template>
  <div class="relative bg-gray-800 p-4 border rounded-lg shadow-md">
    <div class="absolute top-4 right-4 flex space-x-2">
      <UButton
        icon="i-heroicons-pencil-square"
        size="sm"
        color="primary"
        square
        variant="solid"
        @click="handleEdit"
      />
      <UButton
        icon="i-heroicons-trash"
        size="sm"
        class="bg-red-600 text-white hover:bg-red-700"
        square
        variant="solid"
        @click="handleDelete"
      />
    </div>
    <NuxtLink :to="`/details/${post.id}`">
      <div class="text-white">User ID: {{ post.id }}</div>
      <h2 class="mt-4 text-xl font-semibold text-white">
        {{ post.title }}
      </h2>
      <p class="text-white mt-2 w-full truncate">{{ post.body }}</p>
    </NuxtLink>
  </div>
</template>
