<script setup lang="ts">
import { ref } from "vue";
import { type Post } from "../types/Post";
const toast = useToast();

const currentPage = ref<number>(1);
const limit = ref<number>(16);
const totalPosts = ref<number>(0);
const posts = ref<Post[]>([]);
const error = ref<string | null>(null);

const isModalVisible = ref(false);
const isUpdate = ref(false);
const selectedPost = ref<Post>({
  userId: 0,
  id: 0,
  title: "",
  body: "",
});

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

const goToPage = (page: number): void => {
  if (page >= 1 && page <= totalPages) {
    currentPage.value = page;
  }
};

function handleCreate() {
  isModalVisible.value = true;
  isUpdate.value = false;
}

const handleFormSubmit = (formData: Post) => {
  if (isUpdate.value) {
    const id = formData?.id;

    if (id) {
      const index = posts.value.findIndex((post) => post.id === id);

      if (index !== -1) {
        posts.value[index] = { ...posts.value[index], ...formData };
        toast.add({
          title: "Success",
          description: "Post updated successfully!",
        });
      } else {
        toast.add({
          title: "Error",
          description: "Post not found",
        });
      }
    } else {
      toast.add({
        title: "Error",
        description: "Invalid post ID",
      });
    }
  } else {
    const newObj = { ...formData };
    fetch("https://jsonplaceholder.typicode.com/posts", {
      method: "POST",
      body: JSON.stringify({
        title: newObj.title,
        body: newObj.body,
        userId: newObj.userId,
      }),
      headers: {
        "Content-type": "application/json; charset=UTF-8",
      },
    })
      .then((response) => response.json())
      .then((json) => {
        posts.value.push(json);
        toast.add({
          title: "Success",
          description: "Post created successfully!",
        });
      });
  }
};

const handleEdit = (post: Post) => {
  isModalVisible.value = true;
  isUpdate.value = true;
  selectedPost.value = { ...post };
};

const handleDelete = (post: Post) => {
  console.log("delete");
  const id = post.id;
  const index = posts.value.findIndex((post) => post.id === id);
  if (index != -1) {
    posts.value.splice(index, 1);
    toast.add({
      title: "Success",
      description: "Post deleted successfully!",
    });
  } else {
    toast.add({
      title: "Error",
      description: "Error in deleting post",
    });
  }
};
</script>

<template>
  <div class="flex justify-center my-4">
    <UButton color="primary" variant="solid" @click="handleCreate"
      >Create New Post</UButton
    >
  </div>

  <FormModal
    :isVisible="isModalVisible"
    :isUpdate="isUpdate"
    :initialData="selectedPost"
    @close="isModalVisible = false"
    @submit="handleFormSubmit"
  />

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
      <PostCard
        v-for="post in posts"
        :key="post.id"
        :post="post"
        @edit="handleEdit"
        @delete="handleDelete"
      />
    </div>
    <FooterComponent
      :currentPage="currentPage"
      :totalPages="totalPages"
      :goToPage="goToPage"
    />
  </div>
</template>

<style scoped></style>
