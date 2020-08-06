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
    
    <section class="slice-xl page-title has-bg-cover bg-size-cover" style="background-image: url(../../valorant.jpg); background-position: 50% 12%;">
      <div class="container mask-container">
        <div class="row">
          <div class="col-md-6 offset-md-8">
            <div class="">
              <h3 class="heading heading-1 strong-600 text-capitalize mb-1 text-white">
                <span>Conversations</span>
              </h3>
            </div>

            <div class="pb-4 mt-4">
              <p class="lead text-white">
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
            <div class="form-group">
              <input
                v-model="attributeFilter"
                list="attributes"
                type="text"
                class="form-control bg-dark border-dark text-white"
                placeholder="Search"
              />
            </div>
            <datalist id="attributes">
              <option v-for="conversation in conversations">{{ conversation.partner.name }}</option>
            </datalist>
            <div class="card bg-dark border-dark">
              <div class="card-body">
                <table class="table-cart">
                  <thead>
                    <tr>
                      <th class="border-secondary"></th>
                      <th class="border-secondary"></th>
                      <th class="border-secondary"></th>
                      <th class="border-secondary"></th>
                      <th class="border-secondary"></th>
                      <th class="border-secondary"></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="conversation in orderBy(filterBy(conversations, attributeFilter), 'created_at', -1)" class="cart-item border-secondary">
                      <td class="product-image text-center" style="width:33%">
                        <div class="container">
                          <div class='col-md-8'>
                            <img v-if="conversation.partner.image" :src="conversation.partner.image" class="img-responsive" height="50px" style="text-align:center">
                            <img v-else src="/default-user.png" height="50px" width="50px">
                            <br>
                            <div class="text-white">{{conversation.partner.name}}</div>
                          </div>
                        </div>
                      </td>

                      <td class="card-text mt-3" style="width:45%">
                        <div class="container">
                          <div class="row">
                            <p v-if="conversation.last_message" class="text-white" style="display:block; text-overflow:ellipsis; width:220px; overflow:hidden; white-space:nowrap;" >{{conversation.last_message.text}}</p>
                            <p v-else class="text-white" style="display:block; text-overflow:ellipsis; width:220px; overflow:hidden; white-space:nowrap;" >No Messages</p>
                          </div>
                          <div v-if="conversation.last_message" class="row text-muted">
                            {{ relativeDate(conversation.last_message.created_at) }}
                          </div>
                        </div>
                      </td>

                      <td class="product-quantity d-none d-md-table-cell">
                        <div class="input-group input-group--style-2 pr-4" style="width: 130px;">
                          <button v-on:click="showConversation(conversation)" class="btn bg-red" data-toggle="modal" data-target="#conversationModal">Open</button>
                        </div>
                      </td>
                      <td class="product-remove">
                        <a v-on:click="deleteConversation(conversation)" class="text-right pl-4">
                          <i class="ion-trash-a"></i>
                        </a>
                      </td>
                    </tr>

                    <!-- Conversation Modal -->
                    <div class="modal fade" data-backdrop="false" id="conversationModal" tabindex="-1" role="dialog" aria-labelledby="conversationModalLabel" aria-hidden="true">
                      <div class="modal-dialog" role="document">
                        <div class="modal-content bg-dark border-dark">
                          <div class="modal-header border-secondary">
                            <h5 class="modal-title text-white" id="conversationModalLabel">Conversation with {{ partner.name }}</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                              <span class="text-white" aria-hidden="true">&times;</span>
                            </button>
                          </div>
                          <div class="modal-body">
                            <div v-for="message in messages" class="row">
                              <div v-if="partner.name == message.user_name" class="col-md-6">
                                <p class="text-center text-white">{{ message.user_name }}</p>
                                <p class="text-white">{{ message.text }}</p>
                                <p class="text-muted text-center">{{ relativeDate(message.created_at) }}</p>
                              </div>
                              <div v-else class="col-md-6 ml-auto text-right">
                                <p class="text-center text-white">{{ message.user_name }}</p>
                                <p class="text-right text-white">{{ message.text }}</p>
                                <p class="text-muted text-center">{{ relativeDate(message.created_at) }}</p>
                              </div>
                            </div>
                          </div>
                          <div class="modal-footer border-secondary">
                            <div class="container">
                              <div class="row align-items-center">
                                <div class="col-lg-8 text-left">
                                  <textarea class="form-control" v-model="text" placeholder="Message..."></textarea>
                                </div>
                                <div class="col-lg-4 text-center">
                                  <button type="button" class="btn bg-red" v-on:click="createMessage()">Send</button>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </tbody>
                </table>
                
              </div>
            </div>

            <div class="row align-items-center pt-5">
              <div class="col-6">
                <a href="#" class="link link--style-3">
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
      attributeFilter: "",
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
