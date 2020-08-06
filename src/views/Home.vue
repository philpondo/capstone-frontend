<template>
  <div class="home">
    <!-- <h1>{{ message }}</h1>

    <div class="form-group">
      <input v-model="attributeFilter" list ="attributes" type="text" placeholder="Search">
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
        <button type="button" v-on:click="createMessage()">Send</button>
      </div>
    </div>

    <div v-for="post in orderBy(filterBy(posts, attributeFilter), 'created_at', -1)">
      <div>
        <div>
          <h5>{{post.title}}</h5>
        </div>
        <div>
          <img v-if="post.user_image" class="rounded-circle" :src="post.user_image" :alt="post.user_name" width="80" height="80">
          <img v-if="!post.user_image" :src="'../default-user.png'" :alt="post.user_name" width="80" height="80">
          <router-link v-bind:to="`/users/${post.user_id}`">
            <h5>{{post.user_name}}</h5>
          </router-link>
          <p><img v-if="post.user_rank !== 'Unranked'" :src="'../ranks/' + post.user_rank + '.png'" :alt="post.user_rank" width="20" height="20" />{{post.user_rank}} // {{post.user_playstyle}}</p>
        </div>
        <div>
          <p>{{post.content}}</p>
          <p>Players Needed: {{post.players_needed}}</p>
        </div>
        <div v-if="$parent.isLoggedIn()">
          <button v-on:click="createConversation(post)" type="button">Send message</button>
        </div>
        <div>
          <p>Posted {{relativeDate(post.created_at)}}</p>
        </div>
      </div>
    </div> -->
    <!-- <section class="slice page-title has-bg-cover bg-size-cover" style="background-image: url(../../valorant.jpg); background-position: 50% 10%;">
      <div class="container mask-container">
        <div class="row">
          <div class="col-md-6 offset-md-8">
            <div class="">
              <h3 class="heading heading-1 strong-600 text-capitalize mb-1 text-white">
                <span>GoPlay</span>
              </h3>
            </div>
            <div class="pb-4 mt-4">
              <p class="lead text-white">
                Parturient mus aenean a suspendisse est ligula eu dui a ultricies dictumst porttitor rhoncus a vestibulum.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section> -->
    <section class="slice bg-black" style="background-image: url(../../valorant-agents.jpg); background-attachment: fixed;">
      <div class="container mask-container">
        <div class="row">
          <div class="col-md-6">
            <div class="">
              <h3 class="heading heading-1 strong-600 text-capitalize mb-1 text-white">
                <span>GoPlay</span>
              </h3>
            </div>
            <div class="pb-4 mt-4">
              <p class="lead text-white">
                Parturient mus aenean a suspendisse est ligula eu dui a ultricies dictumst porttitor rhoncus a vestibulum.
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="row cols-md-space cols-sm-space cols-xs-space">
          <div class="col-lg-8">
            <div class="card-wrapper">
              <div class="form-group">
                <input
                  v-model="attributeFilter"
                  list="attributes"
                  type="text"
                  class="form-control bg-dark z-depth-2-top border-dark text-white"
                  placeholder="Search"
                />
              </div>
              <!-- <datalist id="attributes">
                <option v-for="post in posts">{{ post.user_rank }}</option>
                <option v-for="post in posts">{{ post.user_name }}</option>
                <option v-for="post in posts">{{ post.user_playstyle }}</option>
                <option v-for="post in posts">{{ post.players_needed }}</option>
              </datalist> -->
              <div
                v-for="post in orderBy(
                  filterBy(posts, attributeFilter),
                  'created_at',
                  -1
                )"
                class="card bg-dark z-depth-2-top"
              >
                <div class="row align-items-center">
                  <div class="col-md-4">
                    <div class="container">
                      <div class="block-author">
                        <div class="author-image author-image-md">
                          <img
                            v-if="post.user_image"
                            :src="post.user_image"
                            :alt="post.user_name"
                          />
                          <img
                            v-else
                            src="/default-user.png"
                            :alt="post.user_name"
                          />
                        </div>
                        <div class="author-info">
                          <span
                            class="d-block heading heading-sm strong-500 mb-0"
                          >
                            <router-link
                              v-bind:to="`users/${post.user_id}`"
                              class="strong-600 text-white"
                              >{{ post.user_name }}</router-link
                            >
                          </span>
                          <span
                            class="d-block heading heading-sm strong-500 mb-0 text-white"
                          >
                            {{ post.user_rank }}
                            <img
                              v-if="post.user_rank !== 'Unranked'"
                              :src="'../ranks/' + post.user_rank + '.png'"
                              :alt="post.user_rank"
                              width="20"
                              height="20"
                            />
                          </span>
                          <span class="d-block text-sm strong-500">
                            {{ post.user_playstyle }}
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="col-md-8">
                    <div class="card-body">
                      <h4 class="heading heading-5 strong-600 line-height-1_8 text-white">
                        {{ post.title }}
                      </h4>
                      <p class="card-text mt-3 text-white">
                        Players needed: {{ post.players_needed }}
                      </p>
                      <p class="card-text mt-3 text-white">
                        {{ post.content }}
                      </p>
                    </div>
                  </div>
                </div>
                <div class="card-footer text-center">
                  <div class="row align-items-center">
                    <div class="col align-items-right">
                      <div class="text-left">
                        <ul class="inline-links inline-links--style-2">
                          <li>
                            Posted {{ relativeDate(post.created_at) }}
                          </li>
                        </ul>
                      </div>
                    </div>
                    <div v-if="$parent.isLoggedIn() && $parent.getUserId() != post.user_id" class="col-md-3 align-items-left">
                      <button
                        v-on:click="createConversation(post)"
                        class="btn btn-md btn-round bg-red"
                        data-toggle="modal" 
                        data-target="#messageModal"
                      >
                        Send Message
                      </button>
                    </div>

                    <div v-if="$parent.isLoggedIn() && $parent.getUserId() == post.user_id" class="col-md-3 align-items-left">
                      <router-link
                        v-bind:to="`/posts/${post.id}/edit`"
                        class="btn btn-md btn-round bg-red"
                      >
                        Edit Post
                      </router-link>
                    </div>
                  </div>
                </div>
              </div>

              <div class="modal fade" data-backdrop="false" id="messageModal" tabindex="-1" role="dialog" aria-labelledby="messageModalLabel" aria-hidden="true">
                <div class="modal-dialog" role="document">
                  <div class="modal-content bg-dark border-dark">
                    <div class="modal-header border-dark">
                      <h5 class="modal-title text-white" id="messageModalLabel">New Message</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span class="text-white" aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      <form>
                        <div class="form-group">
                          <label for="recipient-name" class="col-form-label text-white">Recipient:</label>
                          <input type="text" class="form-control" id="recipient-name" v-model="partner" disabled>
                        </div>
                        <div class="form-group">
                          <label for="message-text" class="col-form-label text-white">Message:</label>
                          <textarea class="form-control" id="message-text" v-model="text" placeholder="Message..."></textarea>
                        </div>
                      </form>
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                      <button type="button" class="btn bg-red" v-on:click="createMessage()">Send message</button>
                    </div>
                  </div>
                </div>
              </div>

            </div>
          </div>

          <!-- <div class="col-lg-3 ml-auto">
            <div class="sidebar">
              <div v-if="showForm == true" class="sidebar-object">
                <div class="section-title section-title--style-1 mb-0">
                  <h3
                    class="section-title-inner heading-sm strong-600 text-uppercase"
                  >
                    New Message to {{ conversation.partner.name }}
                  </h3>
                  <div class="form-group">
                    <textarea
                      class="form-control"
                      v-model="text"
                      placeholder="Message..."
                    ></textarea>
                  </div>
                  <div class="form-group">
                    <button
                      class="btn btn-styled btn-xs btn-red"
                      v-on:click="createMessage()"
                    >
                      Send
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </section>
  </div>
</template>

<style></style>

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
      // showForm: false,
      conversation: {},
      partner: "",
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
        this.partner = this.conversation.partner.name;
        // this.showForm = true;
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
