<template>
<h1>Comment View</h1>
<p>ID: {{tableData.id}}</p>
<h5>Title: {{tableData.title}}</h5>
<h5>Body: {{tableData.body}}</h5>

<br/>
<br/>

<div class="card" v-for="comment in commentData">
  <div class="container">
    <h4><b>{{comment.name}}</b></h4>
    <p>{{comment.email}}</p>
    <p>{{comment.body}}</p>
  </div>
</div>

</template>

<script setup lang="ts">
import { useRoute } from 'vue-router'
import {  ref, onMounted } from 'vue'

const route = useRoute();

const id = route.params.id;
const tableData = ref([])
const commentData = ref([])

onMounted(() => {
  fetchPostData();
  fetchCommentData();
})

const fetchPostData = () => {
  fetch("https://jsonplaceholder.typicode.com/posts/" +id)
    .then((response) => response.json())
    .then((data) => tableData.value = data)
    .catch(error => {
        console.log(error);
    });
}

const fetchCommentData = () => {
  fetch("https://jsonplaceholder.typicode.com/posts/" +id+ "/comments")
    .then((response) => response.json())
    .then((data) => commentData.value = data)
    .catch(error => {
        console.log(error);
    });
}

</script>

<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  border-radius: 5px; /* 5px rounded corners */
}

/* Add rounded corners to the top left and the top right corner of the image */
img {
  border-radius: 5px 5px 0 0;
}
</style>