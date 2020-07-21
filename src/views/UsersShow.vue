<template>
  <div class="users-show">
    <h2>{{user.name}}</h2>
    <p>Rank: {{user.rank}}</p>
    <p>playstyle: {{user.playstyle}}</p>
    <h3>{{user.name}}'s Posts:</h3>
    <div v-for="post in posts">
      <h4>{{post.title}}</h4>
      <p>Players Needed: {{post.players_needed}}</p>
      <p>{{post.content}}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      user: {},
      posts: [],
      errors: []
    };
  },
  created: function() {
    axios.get(`api/users/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.user = response.data;
      this.posts = this.user.posts;
    });
  }
};
</script>