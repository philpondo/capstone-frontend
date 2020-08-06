<template>
  <div class="users-show">

    <section class="slice bg-black" style="background-image: url(../../valorant-agents.jpg); background-attachment: fixed;">
      <div class="container mask-container">
        <div class="row">
          <div class="col-md-6">
            <div class="">
              <h3 class="heading heading-1 strong-600 text-capitalize mb-1 text-white">
                <span>My Profile</span>
              </h3>
            </div>
            <div class="pb-4 mt-4">
              <p class="lead text-white">
              </p>
            </div>
          </div>
        </div>
      </div>

      <div class="profile">
        <div class="container">
          <div class="row cols-xs-space cols-sm-space cols-md-space">
            <div class="col-lg-3">
              <div class="sidebar sidebar--style-2 bg-dark no-border stickyfill">
                <div class="widget">
                  <!-- Profile picture -->
                  <div class="profile-picture profile-picture--style-2">
                    <img
                      v-if="user.image"
                      :src="user.image"
                      :alt="user.name"
                      class="img-center"
                    />
                    <img
                      v-else
                      src="/default-user.png"
                      :alt="user.name"
                      class="img-center"
                    />
                  </div>

                  <!-- Profile details -->
                  <div class="profile-details">
                    <h2 class="heading heading-4 strong-500 profile-name text-white">
                      {{ user.name }}
                    </h2>
                    <h3
                      class="heading heading-6 strong-400 profile-occupation mt-3 text-white"
                    >
                      {{ user.rank }}
                      <img
                        v-if="user.rank !== 'Unranked'"
                        :src="'../ranks/' + user.rank + '.png'"
                        :alt="user.rank"
                        width="20"
                        height="20"
                      />
                    </h3>
                    <h3
                      class="heading heading-light heading-6 strong-400 profile-location text-white"
                    >
                      {{ user.playstyle }}
                    </h3>
                  </div>

                  <!-- Profile edit -->
                  <div class="profile-connect mt-4">
                    <router-link
                      v-if="user.id == $parent.getUserId()"
                      v-bind:to="`${user.id}/edit`"
                      class="btn btn-styled btn-block btn-rounded btn-red"
                    >
                      Edit Profile
                    </router-link>
                  </div>
                </div>
              </div>
            </div>

            <!-- User Posts -->
            <div class="col-lg-8">
              <div class="card-wrapper">
                <div v-if="user.posts.length == 0">
                  <h5 class="text-white">No Posts</h5>
                </div>
                <div v-for="post in orderBy(posts, 'created_at', -1)" class="card bg-dark z-depth-2-top">
                  <div class="row">
                    <div class="col-md-12">
                      <div class="card-header">
                        <h4
                          class="heading heading-5 strong-700 line-height-1_8 text-white"
                        >
                          {{ post.title }}
                        </h4>
                      </div>
                      <div class="card-body">
                        <p class="heading heading-light heading-6 strong-400 profile-location text-white">
                          Players Needed: {{ post.players_needed }}
                        </p>
                        <p class="heading heading-light heading-6 strong-400 profile-location text-white">
                          {{ post.content }}
                        </p>
                      </div>
                      <div class="card-footer">
                        <div class="row">
                          <div class="col-6">
                            <router-link
                              v-if="user.id == $parent.getUserId()"
                              v-bind:to="`/posts/${post.id}/edit`"
                              class="btn btn-sm btn-rounded btn-red"
                            >
                              Edit Post
                            </router-link>
                          </div>
                          <div class="col-6">
                            <div class="text-right">
                              <ul class="inline-links inline-links--style-2">
                                <li>
                                  Posted {{ relativeDate(post.created_at) }}
                                </li>
                              </ul>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
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
import moment from "moment";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
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
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
  },
};
</script>
