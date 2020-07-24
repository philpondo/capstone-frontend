<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="post in posts">
      <h4>{{post.title}}</h4>
      <router-link v-bind:to="`/users/${post.user_id}`">
        <p>{{post.user_name}}</p>
      </router-link>
      <p>{{post.user_rank}} | {{post.user_playstyle}}</p>
      <p>Players Needed: {{post.players_needed}}</p>
      <p>{{post.content}}</p>
      <button>Send message</button>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "GoPlay",
      posts: [],
    };
  },
  created: function () {
    axios.get("/api/posts").then((response) => {
      console.log("All Posts:", response.data);
      this.posts = response.data;
    });
  },
  methods: {},
};
</script>
