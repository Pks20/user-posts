<script setup lang="ts">
defineProps<{
  currentPage: number;
  totalPages: number;
}>();

</script>

<template>
  <footer class="mt-8 flex justify-center items-center space-x-4">
    <button
      @click="$emit('changePage',currentPage-1)"
      :disabled="currentPage === 1"
      class="px-4 py-2 bg-gray-600 text-white rounded hover:bg-gray-700 disabled:opacity-50"
    >
      Previous
    </button>

    <span class="text-gray-800">
      Page {{ currentPage }} of {{ totalPages }}
    </span>

    <button
      @click="$emit('changePage',currentPage+1)"
      :disabled="currentPage === totalPages"
      class="px-4 py-2 bg-gray-600 text-white rounded hover:bg-gray-700 disabled:opacity-50"
    >
      Next
    </button>
  </footer>
</template>

<style scoped></style>
