<script setup lang="ts">
import { type Post } from "../types/Post";
const props = defineProps<{
  isVisible: boolean;
  isUpdate: boolean;
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
  formData.value = {
    userId: 0,
    id: 0,
    title: "",
    body: "",
  };
};

const closeModal = () => {
  resetForm();
  emit("close");
};
</script>

<template>
  <UModal v-model="props.isVisible">
    <UContainer class="fixed inset-0 flex items-center justify-center">
      <UCard class="bg-white rounded-lg shadow-xl p-6 w-full max-w-lg">
        <template #header>
          <h2 class="text-xl font-bold mb-4"> {{ isUpdate ? "Update Post" : "Create New Post" }}</h2>
        </template>
        <UForm :state="formData" @submit.prevent="handleSubmit">
          <UFormGroup name="userId" label="User Id">
            <UInput type="number" v-model="formData.userId" required/>
          </UFormGroup>
          <UFormGroup name="title" label="Title">
            <UInput type="text" v-model="formData.title" required/>
          </UFormGroup>
          <UFormGroup name="body" label="Body">
            <UTextarea v-model="formData.body" />
          </UFormGroup>
          <UButton class="mt-4 mr-3" @click="closeModal">Cancel</UButton>
          <UButton type="submit">{{ isUpdate ? "Update" : "Create" }}</UButton>
        </UForm>
      </UCard>
    </UContainer>
  </UModal>
</template>

<style scoped></style>
