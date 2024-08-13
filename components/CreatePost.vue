<script setup lang="ts">
import { useMutation } from '@tanstack/vue-query';
import type { CustomError } from '~/type';

const form = reactive<{
  id: number | null
  title: string;
  description: string
}>({
  id: null,
  title: '',
  description: ''
})

const message = ref<{ id: number } | null>(null)

const createPost = async (body: {
  id: number
  title: string
  description: string
}) => {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    body: JSON.stringify(body),
    headers: {
      "Content-Type": "application/json",
    },
  })

  message.value = await response.json()
}

const { isPending, isError, isSuccess, error, mutate } = useMutation({
  mutationFn: createPost,
  retry: 3,
})

const addPost = () => {
  mutate({ ...form, id: Date.now() })
}
</script>
<template>
  <div class="post space-y-4 mb-5">
    <h1 class="mb-5">Create a Post</h1>
    <div class="space-x-5">
      <label>Title:</label>
      <input class="bg-black border border-white" type="text" v-model="form.title" />
    </div>

    <br>

    <div class="space-x-5">
      <label>Description:</label>
      <input class="bg-black border border-white" type="text" v-model="form.description" />
    </div>

    <br>
    <button @click="addPost" class="bg-blue-600 rounded-md px-6 py-1">
      Create
    </button>
    <p v-if="isSuccess"> Created a new Post ID: {{ message && message.id }}</p>
    <div style="background: '#234'; color: gray;">
      {{ isPending ? "Saving..." : "" }}
      {{ isError ? (error as CustomError).message : "" }}
    </div>
  </div>
</template>