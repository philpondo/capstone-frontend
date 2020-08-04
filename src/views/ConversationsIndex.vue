<template>
  <div class="conversations-index">
    <!-- <div v-if="conversations.length == 0">
      <h5>No Conversations</h5>
    </div>
    <div v-for="conversation in orderBy(conversations, 'created_at', -1)">
      <div v-if="conversation.last_message">
        <h5>Conversation with {{ conversation.partner.name }}</h5>
        <p v-if="conversation.last_message">
          Last Message: {{ conversation.last_message.text }}
        </p>
        <p>{{ relativeDate(conversation.last_message.created_at) }}</p>
        <button v-on:click="showConversation(conversation)" type="button">
          Open
        </button>
        <button v-on:click="deleteConversation(conversation)" type="button">
          Delete
        </button>
      </div>
    </div>
    <div v-if="currentConversation">
      <h5>Conversation with {{ partner.name }}</h5>
      <div>
        <div v-for="message in messages">
          <p>{{ message.user_name }}:</p>
          <p>{{ message.text }}</p>
          <p>{{ relativeDate(message.created_at) }}</p>
        </div>
      </div>
      <div>
        <div>
          <textarea v-model="text" placeholder="Message..."></textarea>
        </div>
      </div>
      <div>
        <button type="button" v-on:click="createMessage()">Send</button>
        <button type="button" v-on:click="currentConversation = ''">
          Close
        </button>
      </div>
    </div> -->
    
    <section class="slice-xl page-title has-bg-cover bg-size-cover" style="background-image: url(../../valorant.jpg); background-position: 50% 15%;">
      <div class="container mask-container">
        <div class="row">
          <div class="col-md-6 offset-md-8">
            <div class="">
              <h3 class="heading heading-1 strong-600 text-capitalize mb-1 text-white">
                <span>Conversations</span>
              </h3>
            </div>

            <!-- Fluid text paragraph -->
            <div class="pb-4 mt-4">
              <p class="lead text-white">
                Parturient mus aenean a suspendisse est ligula eu dui a ultricies dictumst porttitor rhoncus a vestibulum.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="slice bg-black">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-8 mt--150">
            <div class="card">
              <div class="card-body">
                <table class="table-cart">
                  <thead>
                    <tr>
                      <th></th>
                      <th></th>
                      <th></th>
                      <th></th>
                      <th></th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="conversation in orderBy(conversations, 'created_at', -1)" class="cart-item">
                      <td class="product-image text-center" style="width:33%">
                        <div class="container">
                          <div class='col-md-8'>
                            <img v-if="conversation.partner.image" :src="conversation.partner.image" height="50px" style="text-align:center">
                            <img v-else src="/default-user.png" height="50px" width="50px">
                            <br>
                            {{conversation.partner.name}}
                          </div>
                        </div>
                      </td>

                      <td class="card-text mt-3" style="width:45%">
                        <div class="container">
                          <div class="row">
                            <p style="display:block; text-overflow:ellipsis; width:220px; overflow:hidden; white-space:nowrap;">{{conversation.last_message.text}}</p>
                          </div>
                          <div class="row text-muted">
                            {{ relativeDate(conversation.last_message.created_at) }}
                          </div>
                        </div>
                      </td>

                      <td class="product-quantity d-none d-md-table-cell">
                        <div class="input-group input-group--style-2 pr-4" style="width: 130px;">
                          <button v-on:click="showConversation(conversation)" class="btn btn-primary">Open</button>
                        </div>
                      </td>
                      <td class="product-remove">
                        <a v-on:click="deleteConversation(conversation)" class="text-right pl-4">
                          <i class="ion-trash-a"></i>
                        </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>

            <div class="row align-items-center pt-5">
              <div class="col-6">
                <a href="#" class="link link--style-3">
                    <i class="ion-android-arrow-back"></i>
                    Return to shop
                </a>
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
      conversations: [],
      currentConversation: {},
      messages: [],
      partner: {},
      text: "",
      conversationId: "",
    };
  },
  created: function () {
    axios.get("/api/conversations").then((response) => {
      console.log("All Conversations:", response.data);
      this.conversations = response.data;
    });
  },
  methods: {
    showConversation: function (conversation) {
      axios.get(`/api/conversations/${conversation.id}`).then((response) => {
        console.log("Conversation:", response.data);
        this.currentConversation = response.data;
        this.messages = this.currentConversation.messages;
        this.partner = this.currentConversation.partner;
      });
    },
    deleteConversation: function (conversation) {
      if (confirm("Are you sure you want to delete this conversation?")) {
        axios
          .delete(`/api/conversations/${conversation.id}`)
          .then((response) => {
            console.log("Successfully destroyed", response.data);
            // splice out of conversations array
            this.conversations.splice(
              this.conversations.indexOf(conversation),
              1
            );
          });
      }
    },
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    createMessage: function () {
      var formData = new FormData();
      formData.append("text", this.text);
      formData.append("conversation_id", this.currentConversation.id);
      axios.post("/api/messages", formData).then((response) => {
        this.showConversation(this.currentConversation);
        this.text = "";
      });
    },
  },
};
</script>
