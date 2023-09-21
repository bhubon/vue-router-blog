<script setup>
import { reactive, ref, onBeforeMount } from 'vue';

const posts = ref([]);

onBeforeMount(() => {
    fetch('https://dummyjson.com/posts?limit=20')
        .then(res => res.json())
        .then(data => {
            posts.value = data.posts
        })
});

</script>
<template>
    <article class="mb-10" v-for="post in posts" :key="post.id">
        <h1 class="text-xl mb-2">
            <router-link :to="{ name: 'post', params: { id: post.id } }">{{ post.title }}</router-link>
        </h1>
        <p>
            <router-link :to="{ name: 'post', params: { id: post.id } }"><img
                :src="`https://source.unsplash.com/random/300x200?${post.id}`" alt=""></router-link>
            {{ post.body }}
        </p>
    </article>
</template>

<style scoped></style>