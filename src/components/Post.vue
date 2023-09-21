<script setup>
import { useRoute } from 'vue-router';
import { reactive, ref, onBeforeMount } from 'vue';

const post = reactive({});
const route = useRoute();
const id = route.params.id;
const comments = ref([]);


onBeforeMount(() => {
    fetch('https://dummyjson.com/posts/' + id)
        .then(res => res.json())
        .then(data => {
            post.id = data.title;
            post.body = data.body;
            post.title = data.title;
        });

    fetch(`https://dummyjson.com/posts/${id}/comments`)
        .then(res => res.json())
        .then(data => {
            comments.value = data.comments
        });
});



</script>
<template>
    <article class="mb-10">
        <h1 class="text-xl mb-2">{{ post.title }}</h1>
        <p>
            <img :src="`https://source.unsplash.com/random/300x200?${post.id}`" alt="">
            {{ post.body }}
        </p>
    </article>
    <article>
        <h2 class="font-bold underline-gray-600 mb-2" v-if="comments.length > 0">Comments</h2>
        <hr />
        <ul class="mt-5">
            <li v-for="comment in comments" :key="comment.id">
                <p><strong>{{ comment.user.username }}</strong> said {{ comment.body }}</p>
            </li>
        </ul>
    </article>
</template>

<style scoped></style>