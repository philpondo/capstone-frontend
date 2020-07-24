<template>
  <div class="posts-new">
    <div class="container">
      <div class="row">
        <div class="col">
          <div class="card text-center text-white bg-dark mb-3">
            <form v-on:submit.prevent="createPost()">
              <div class="card-header">
                <h4>New Post</h4>
              </div>
              <div class="card-body">
                <div class="container">
                  <ul>
                    <li class="text-danger" v-for="error in errors">{{ error }}</li>
                  </ul>
                  <div class="form-group">
                    <label class="col-form-label">Title:</label>
                    <input type="text" class="form-control" v-model="title" />
                  </div>
                  <div class="form-group">
                    <label>Players Needed:</label>
                    <select class="form-control" v-model="playersNeeded">
                      <option>1</option>
                      <option>2</option>
                      <option>3</option>
                      <option>4</option>
                    </select>
                  </div>
                  <div class="form-group">
                    <label>Content:</label>
                    <textarea class="form-control" v-model="content"></textarea> 
                  </div>
                </div>
              </div>
              <div class="card-footer">
                <input type="submit" class="btn btn-primary" value="Create" />
              </div>
            </form>
          </div>
        </div>
      </div>
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