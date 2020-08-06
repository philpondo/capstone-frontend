<template>
  <div class="posts-new">
    <!-- <div>
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
    </div> -->

    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="background-image: url(../../valorant.jpg); background-position: top center;"
    >
      <div class="container">
        <div class="row">
          <div class="col-lg-6 offset-lg-3">
            <div class="card bg-dark border-dark">
              <div class="card-title">
                <h5 class="heading heading-5 strong-500 text-white">
                  Create a new post
                </h5>
              </div>
              <div class="card-body bg-dark">
                <form
                  class="form-default"
                  role="form"
                  v-on:submit.prevent="createPost()"
                >
                  <div class="row">
                    <div class="col-12">
                      <div class="form-group">
                        <label class="text-white">Title</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="title"
                        />
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-lg-12">
                      <div class="form-group">
                        <label class="text-white">Players Needed:</label>
                        <select class="form-control" v-model="playersNeeded">
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
                          v-model="content"
                        ></textarea>
                      </div>
                    </div>
                  </div>
                  <button
                    type="submit"
                    class="btn btn-styled btn-lg btn-block bg-red mt-4"
                    value="Create"
                  >
                    Submit
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
