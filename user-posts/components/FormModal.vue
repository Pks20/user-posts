<script setup lang="ts">
import { type Post } from "../types/Post";
const props = defineProps<{
  isVisible: Boolean;
  isUpdate: Boolean;
  initialData: Post;
}>();

const emit = defineEmits(["close", "submit"]);

const formData = ref({ ...props.initialData });

watch(
  () => props.initialData,
  (newData) => {
    if (newData) {
      formData.value = { ...newData };
    }
  },
  { immediate: true }
);

const handleSubmit = () => {
  emit("submit", formData.value);
  resetForm();
  closeModal();
};

const resetForm = () => {
  formData.value = { ...props.initialData };
};

const closeModal = () => {
  resetForm();
  emit("close");
};
</script>

<template>
  <div
    v-if="isVisible"
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50"
  >
    <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-lg">
      <h2 class="text-xl font-bold mb-4">
        {{ isUpdate ? "Update Post" : "Create New Post" }}
      </h2>

      <form @submit.prevent="handleSubmit">
        <div class="mb-4">
          <label for="userId" class="block text-sm font-medium text-gray-700"
            >User ID</label
          >
          <input
            type="number"
            id="userId"
            v-model="formData.userId"
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
          />
        </div>

        <div class="mb-4">
          <label for="title" class="block text-sm font-medium text-gray-700"
            >Title</label
          >
          <input
            type="text"
            id="title"
            v-model="formData.title"
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
          />
        </div>

        <div class="mb-4">
          <label for="body" class="block text-sm font-medium text-gray-700"
            >Body</label
          >
          <textarea
            id="body"
            required
            v-model="formData.body"
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
          ></textarea>
        </div>

        <div class="flex justify-end space-x-4">
          <button
            type="button"
            @click="closeModal"
            class="bg-gray-500 text-white px-4 py-2 rounded-md hover:bg-gray-600"
          >
            Cancel
          </button>
          <button
            type="submit"
            class="bg-indigo-500 text-white px-4 py-2 rounded-md hover:bg-indigo-600"
          >
            {{ isUpdate ? "Update" : "Create" }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped></style>
