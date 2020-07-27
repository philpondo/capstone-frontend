<template>
  <div class="users-show">
    <h2>{{user.name}}</h2>
    <p>Rank: {{user.rank}}</p>
    <p>Playstyle: {{user.playstyle}}</p>
    <div v-if="user.id == $parent.getUserId()">
      <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#editUserModal">Edit</button>
    </div>
    <!-- Edit User Modal -->
    <div class="modal fade" id="editUserModal" tabindex="-1" role="dialog" aria-labelledby="editUserModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editUserModalLabel">Edit User</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="editUser()">
              <ul>
                <li class="text-danger" v-for="error in errors">{{ error }}</li>
              </ul>
              <div class="form-group">
                <label>Name:</label> 
                <input type="text" class="form-control" v-model="user.name">
              </div>
              <div class="form-group">
                <label>Email:</label>
                <input type="email" class="form-control" v-model="user.email">
              </div>
              <div class="form-group">
                <label>Rank:</label> 
                <select class="form-control" v-model="user.rank">
                  <option>Unranked</option>
                  <option>Iron I</option>
                  <option>Iron II</option>
                  <option>Iron III</option>
                  <option>Bronze I</option>
                  <option>Bronze II</option>
                  <option>Bronze III</option>
                  <option>Silver I</option>
                  <option>Silver II</option>
                  <option>Silver III</option>
                  <option>Gold I</option>
                  <option>Gold II</option>
                  <option>Gold III</option>
                  <option>Platinum I</option>
                  <option>Platinum II</option>
                  <option>Platinum III</option>
                  <option>Diamond I</option>
                  <option>Diamond II</option>
                  <option>Diamond III</option>
                  <option>Immortal I</option>
                  <option>Immortal II</option>
                  <option>Immortal III</option>
                  <option>Radiant</option>
                </select>
              </div>
              <div class="form-group">
                <label>Playstyle:</label> 
                <select class="form-control" v-model="user.playstyle">
                  <option>beginner</option>
                  <option>casual</option>
                  <option>competitive</option>
                </select>
              </div>
              <input type="submit" class="btn btn-primary" value="Update">
              <button v-on:click="destroyUser()" type="button" class="btn btn-primary">Delete</button>
            </form>
          </div>
        </div>
      </div>
    </div>
    <h3>{{user.name}}'s Posts:</h3>
    <div v-if="posts.length == 0">
      <h5>No Posts</h5>
    </div>
    <div v-for="post in posts">
      <h4>{{post.title}}</h4>
      <p>Players Needed: {{post.players_needed}}</p>
      <p>{{post.content}}</p>
      <div v-if="post.user_id == $parent.getUserId()">
        <button v-on:click="showPost(post)" type="button" class="btn btn-primary" data-toggle="modal" data-target="#editPostModal">Edit</button>
        <button v-on:click="destroyPost(post)" type="button" class="btn btn-primary">Delete</button>
      </div>
    </div>
    <!-- Edit Post Modal -->
    <div class="modal fade" id="editPostModal" tabindex="-1" role="dialog" aria-labelledby="editPostModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editPostModalLabel">Edit Post</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="editPost(currentPost)">
              <ul>
                <li class="text-danger" v-for="error in errors">{{ error }}</li>
              </ul>
              <div class="form-group">
                <label class="col-form-label">Title:</label>
                <input type="text" class="form-control" v-model="currentPost.title" />
              </div>
              <div class="form-group">
                <label>Players Needed: {{currentPost.players_needed}}</label>
                <select class="form-control" v-model="currentPost.playersNeeded">
                  <option>1</option>
                  <option>2</option>
                  <option>3</option>
                  <option>4</option>
                </select>
              </div>
              <div class="form-group">
                <label>Content:</label>
                <textarea class="form-control" v-model="currentPost.content"></textarea> 
              </div>
              <input type="submit" class="btn btn-primary" value="Update">
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
      user: {},
      posts: [],
      errors: [],
      currentPost: {},
      name: "",
      email: "",
      rank: "",
      playstyle: "",
    };
  },
  created: function () {
    axios.get(`api/users/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.user = response.data;
      this.posts = this.user.posts;
    });
  },
  methods: {
    showPost: function (post) {
      axios.get(`/api/posts/${post.id}`).then((response) => {
        console.log("Post:", response.data);
        this.currentPost = response.data;
      });
    },
    destroyPost: function (post) {
      if (confirm("Are you sure you want to delete this post?")) {
        axios.delete(`/api/posts/${post.id}`).then((response) => {
          console.log("Successfully destroyed", response.data);
          window.location.reload();
        });
      }
    },
    editPost: function (post) {
      var params = {
        title: post.title,
        players_needed: post.players_needed,
        content: post.content,
      };
      axios
        .patch(`/api/posts/${post.id}`, params)
        .then((response) => {
          $("#editPostModal").modal("hide");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
      window.location.reload();
    },
    destroyUser: function () {
      if (confirm("Are you sure you want to delete your account?")) {
        axios.delete(`/api/users/${this.user.id}`).then((response) => {
          console.log("Successfully destroyed", response.data);
          window.location.reload();
        });
      }
    },
    editUser: function () {
      var params = {
        name: this.user.name,
        email: this.user.email,
        rank: this.user.rank,
        playstyle: this.user.playstyle,
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
        .then((response) => {
          $("#editUserModal").modal("hide");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
      window.location.reload();
    },
  },
};
</script>