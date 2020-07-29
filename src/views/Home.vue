<template>

  <div class="home">
    <div class="container">

      <h1>{{ message }}</h1>

      <div class="form-group">
        <input class="form-control" v-model="attributeFilter" list ="attributes" type="text" placeholder="Search">
      </div>
      <datalist id="attributes">
        <option v-for="post in posts">{{post.user_rank}}</option>
        <option v-for="post in posts">{{post.user_name}}</option>
        <option v-for="post in posts">{{post.user_playstyle}}</option>
        <option v-for="post in posts">{{post.players_needed}}</option>
      </datalist>

      <div v-if="showForm == true">
        <h5>New Message to {{conversation.partner.name}}</h5>
        <div class="form-group">
          <textarea class="form-control" v-model="text" placeholder="Message..."></textarea> 
        </div>
        <div class="form-group">
          <button type="button" class="btn btn-primary" v-on:click="createMessage()">Send</button>
        </div>
      </div>

      <div v-for="post in orderBy(filterBy(posts, attributeFilter), 'created_at', -1)">
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
                      <img v-if="post.user_image" class="rounded-circle" :src="post.user_image" :alt="post.user_image" width="80" height="80">
                      <router-link v-bind:to="`/users/${post.user_id}`">
                        <h5>{{post.user_name}}</h5>
                      </router-link>
                      <p><img v-if="post.user_rank !== 'Unranked'" :src="'../ranks/' + post.user_rank + '.png'" :alt="post.user_rank" width="20" height="20" />{{post.user_rank}} // {{post.user_playstyle}}</p>
                    </div>
                    <div class="col-md-6">
                      <p>{{post.content}}</p>
                      <p>Players Needed: {{post.players_needed}}</p>
                    </div>
                    <div v-if="$parent.isLoggedIn()" class="col-sm-3">
                      <button v-on:click="createConversation(post)" type="button" class="btn btn-primary">Send message</button>
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      message: "GoPlay",
      posts: [],
      showForm: false,
      conversation: {},
      text: "",
      attributeFilter: "",
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
    createConversation: function (post) {
      var params = {
        recipient_id: post.user_id,
        post_id: post.id,
      };
      axios.post("/api/conversations", params).then((response) => {
        console.log("New conversation created:", response.data);
        this.conversation = response.data;
        this.showForm = true;
      });
    },
    createMessage: function () {
      var formData = new FormData();
      formData.append("text", this.text);
      formData.append("conversation_id", this.conversation.id);
      axios.post("/api/messages", formData).then((response) => {
        this.text = "";
        this.showForm = false;
        this.$router.push("/conversations");
      });
    },
  },
};
</script>
