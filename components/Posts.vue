<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query';
import type { Post } from '~/type';

const getPosts = async (): Promise<Post[]> => {
  const data = await fetch("https://jsonplaceholder.typicode.com/posts")
  return data.json()
}

const { data: posts, suspense } = useQuery({
  queryKey: ['posts'],
  queryFn: getPosts,
})

onServerPrefetch(async () => {
  await suspense()
})
</script>
<template>
  <div>
    <h1>All Posts</h1>
    <li v-for="post in posts" class="mt-3 border-b border-blue-500" :key=post.id>
      {{ post.title }}
    </li>
  </div>
</template>