<template>
  <div class="users-show">

    <img v-if="user.image" :src="user.image" :alt="user.name" width="250" height="200">
    <h2>{{user.name}}</h2>
    <p>Rank: {{user.rank}} <img v-if="user.rank !== 'Unranked'" :src="'../ranks/' + user.rank + '.png'" :alt="user.rank" width="20" height="20" /></p>
    <p>Playstyle: {{user.playstyle}}</p>
    <div v-if="user.id == $parent.getUserId()">
      <button type="button">Edit</button>
    </div>

    <div>
      <h5>Edit User</h5>
    </div>
    <div>
      <form v-on:submit.prevent="editUser()">
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
        <div>
          <label>Name:</label> 
          <input type="text" v-model="user.name">
        </div>
        <div>
          <label>Email:</label>
          <input type="text" v-model="user.email">
        </div>
        <div>
          <label>New Password:</label>
          <input type="password" v-model="password">
        </div>
        <div>
          <label>Password Confirmation:</label>
          <input type="password" v-model="passwordConfirmation">
        </div>
        <div>
          <label>Rank:</label> 
          <select v-model="user.rank">
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
        <div>
          <label>Playstyle:</label> 
          <select v-model="user.playstyle">
            <option>beginner</option>
            <option>casual</option>
            <option>competitive</option>
          </select>
        </div>
        <div>
          <label>Profile Picture:</label>
          <input type="file" v-on:change="setFile($event)" ref="fileInput">
        </div>
        <input type="submit" value="Update">
        <button v-on:click="destroyUser()" type="button">Delete</button>
      </form>
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
        <button v-on:click="showPost(post)" type="button">Edit</button>
        <button v-on:click="destroyPost(post)" type="button">Delete</button>
      </div>
    </div>

    <div>
      <h5>Edit Post</h5>
    </div>
    <div>
      <form v-on:submit.prevent="editPost(currentPost)">
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
        <div>
          <label>Title:</label>
          <input type="text" v-model="currentPost.title" />
        </div>
        <div>
          <label>Players Needed: {{currentPost.players_needed}}</label>
          <select v-model="currentPost.playersNeeded">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
          </select>
        </div>
        <div>
          <label>Content:</label>
          <textarea v-model="currentPost.content"></textarea> 
        </div>
        <input type="submit" value="Update">
      </form>
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
      image: "",
      password: "",
      passwordConfirmation: "",
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
    setFile: function (event) {
      if (event.target.files.length > 0) {
        this.user.image = event.target.files[0];
      }
    },
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
          // splice post out of posts array
          this.posts.splice(this.posts.indexOf(post), 1);
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
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyUser: function () {
      if (confirm("Are you sure you want to delete your account?")) {
        axios.delete(`/api/users/${this.user.id}`).then((response) => {
          console.log("Successfully destroyed", response.data);
          this.$router.push("/");
        });
      }
    },
    editUser: function () {
      var formData = new FormData();
      formData.append("name", this.user.name);
      formData.append("email", this.user.email);
      formData.append("rank", this.user.rank);
      formData.append("playstyle", this.user.playstyle);
      if (this.user.image) {
        formData.append("image", this.user.image);
      }
      if (this.password && this.passwordConfirmation) {
        formData.append("password", this.user.password);
        formData.append("passwordConfirmation", this.user.passwordConfirmation);
      }
      axios
        .patch(`/api/users/${this.user.id}`, formData)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>