<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>New Post</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Players Needed:</label>
        <input type="text" class="form-control" v-model="playersNeeded" />
      </div>
      <div class="form-group">
        <label>Content:</label>
        <input type="text" class="form-control" v-model="content" />
      </div>
      <input type="submit" class="btn btn-primary" value="Create" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      title: "",
      playersNeeded: "",
      content: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    createPost: function() {
      var params = {
        title: this.title,
        players_needed: this.playersNeeded,
        content: this.content
      };
      axios
        .post("api/posts", params)
        .then(response => {
          this.$router.push("/");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>