<template>
  <div class="posts-edit">

    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="background-image: url(../../valorant.jpg); background-position: top center;"
    >
      <div class="container">
        <div class="row">
          <div class="col-lg-6 offset-lg-3">
            <div class="card bg-dark border-dark">
              <div class="card-title text-center">
                <h5 class="heading heading-5 strong-500 text-white">
                  Edit Post
                </h5>
              </div>
              <div class="card-body">
                <form
                  class="form-default"
                  role="form"
                  v-on:submit.prevent="editPost()"
                >
                  <div class="row">
                    <div class="col-12">
                      <div class="form-group">
                        <label class="text-white">Title</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="post.title"
                        />
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-lg-12">
                      <div class="form-group">
                        <label class="text-white">Players Needed:</label>
                        <select class="form-control" v-model="post.players_needed">
                          <option>1</option>
                          <option>2</option>
                          <option>3</option>
                          <option>4</option>
                        </select>
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-lg-12">
                      <div class="form-group">
                        <label class="text-white" for="contentTextarea">Content</label>
                        <textarea
                          class="form-control"
                          id="contentTextarea"
                          rows="3"
                          v-model="post.content"
                        ></textarea>
                      </div>
                    </div>
                  </div>
                  <button
                    type="submit"
                    class="btn btn-styled btn-lg btn-block bg-red mt-4"
                    value="Create"
                  >
                    Update
                  </button>
                  <button
                    v-on:click="destroyUser()"
                    class="btn btn-styled btn-lg btn-block bg-secondary mt-4 text-white"
                  >
                    Delete
                  </button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
      title: "",
      playersNeeded: "",
      content: "",
      errors: [],
    };
  },
  created: function () {
    axios.get(`/api/posts/${this.$route.params.id}`).then((response) => {
      console.log("Post:", response.data);
      this.post = response.data;
    });
  },
  methods: {
    editPost: function (post) {
      var params = {
        title: this.post.title,
        players_needed: this.post.players_needed,
        content: this.post.content,
      };
      axios
        .patch(`/api/posts/${this.post.id}`, params)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/users/${this.post.user_id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyPost: function (post) {
      if (confirm("Are you sure you want to delete this post?")) {
        axios.delete(`/api/posts/${this.post.id}`).then((response) => {
          console.log("Successfully destroyed", response.data);
          // splice post out of posts array
          this.posts.splice(this.posts.indexOf(post), 1);
        });
      }
    },
  },
};
</script>