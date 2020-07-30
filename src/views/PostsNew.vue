<template>
  <div class="posts-new">
    <div>
      <form v-on:submit.prevent="createPost()">
        <div>
          <h4>New Post</h4>
        </div>
        <div>
          <ul>
            <li v-for="error in errors">{{ error }}</li>
          </ul>
          <div>
            <label>Title:</label>
            <input type="text" v-model="title" />
          </div>
          <div>
            <label>Players Needed:</label>
            <select v-model="playersNeeded">
              <option>1</option>
              <option>2</option>
              <option>3</option>
              <option>4</option>
            </select>
          </div>
          <div>
            <label>Content:</label>
            <textarea v-model="content"></textarea> 
          </div>
        </div>
        <div class="card-footer">
          <input type="submit" value="Create" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      title: "",
      playersNeeded: "",
      content: "",
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createPost: function () {
      var params = {
        title: this.title,
        players_needed: this.playersNeeded,
        content: this.content,
      };
      axios
        .post("api/posts", params)
        .then((response) => {
          this.$router.push("/");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>