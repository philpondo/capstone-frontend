<template>
  <div class="home">
    <div class="container">
      <h1>{{ message }}</h1>
      <div v-for="post in posts">
        <div class="row">
          <div class="col">
            <div class="card text-center text-white bg-dark mb-3">
              <div class="card-header">
                <h5>{{post.title}}</h5>
              </div>
              <div class="card-body">
                <div class="container">
                  <div class="row align-items-center">
                    <div class="col-sm-3">
                      <router-link v-bind:to="`/users/${post.user_id}`">
                        <h5>{{post.user_name}}</h5>
                      </router-link>
                      <p>{{post.user_rank}} // {{post.user_playstyle}}</p>
                    </div>
                    <div class="col-md-6">
                      <p>{{post.content}}</p>
                      <p>Players Needed: {{post.players_needed}}</p>
                    </div>
                    <div v-if="$parent.isLoggedIn()" class="col-sm-3">
                      <button type="button" class="btn btn-primary">Send message</button>
                    </div>
                  </div>
                </div>
              </div>
              <div class="card-footer text-muted">
                <p>Posted {{relativeDate(post.created_at)}}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import moment from "moment";
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
  methods: {
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
  },
};
</script>
