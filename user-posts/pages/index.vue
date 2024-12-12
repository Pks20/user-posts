<script setup lang="ts">
import { ref } from "vue";

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const currentPage = ref<number>(1);
const limit = ref<number>(16);
const totalPosts = ref<number>(0);
const posts = ref<Post[]>([]);
const error = ref<string | null>(null);

const fetchPosts = async () => {
  try {
    const { data, error: fetchError } = await useFetch<Post[]>(
      `https://jsonplaceholder.typicode.com/posts?_page=${currentPage.value}&_limit=${limit.value}`
    );

    if (data.value) {
      posts.value = data.value;
      totalPosts.value = data.value?.length;
    }

    if (fetchError.value) {
      error.value = fetchError.value.message;
      return;
    }
  } catch (err) {
    console.log(err);
  }
};

const { data: totalData } = await useFetch<Post[]>(
  "https://jsonplaceholder.typicode.com/posts"
);

watch([currentPage, limit], fetchPosts, { immediate: true });

if (totalData.value) {
  totalPosts.value = totalData.value?.length;
}

const totalPages = Math.ceil(totalPosts.value / limit.value);

console.log(totalPosts.value);

const goToPage = (page: number): void => {
  if (page >= 1 && page <= totalPages) {
    currentPage.value = page;
  }
};
</script>

<template>
  <div v-if="error" class="text-red-500">
    Failed to fetch posts: {{ error }}
  </div>
  <div
    v-else
    class="container mx-auto p-4 min-h-screen flex flex-col justify-between"
  >
    <div
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8"
    >
      <PostCard v-for="post in posts" :key="post.id" :post="post" />
    </div>
    <!-- <footer class="mt-8 flex justify-center items-center space-x-4">
      <button
        @click="goToPage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="px-4 py-2 bg-gray-600 text-white rounded hover:bg-gray-700 disabled:opacity-50"
      >
        Previous
      </button>

      <span class="text-gray-800">
        Page {{ currentPage }} of {{ totalPages }}
      </span>

      <button
        @click="goToPage(currentPage + 1)"
        :disabled="currentPage === totalPages"
        class="px-4 py-2 bg-gray-600 text-white rounded hover:bg-gray-700 disabled:opacity-50"
      >
        Next
      </button>
    </footer> -->

    <FooterComponent
      :currentPage="currentPage"
      :totalPages="totalPages"
      :goToPage="goToPage"
    />
  </div>
</template>

<style scoped></style>
