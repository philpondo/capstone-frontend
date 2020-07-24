<template>
  <div class="conversations-index">
    <div v-for="conversation in conversations">
      <h5>Conversation with {{conversation.partner.name}}</h5>
      <p>Last Message: {{conversation.last_message.text}}</p>
      <!-- Button trigger modal -->
      <button v-on:click="showConversation(conversation)" type="button" class="btn btn-primary" data-toggle="modal" data-target="#showConversationModal">
        Open
      </button>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="showConversationModal" tabindex="-1" role="dialog" aria-labelledby="showConversationModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="showConversationModalLabel">Conversation with {{partner.name}}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="container-fluid">
              <div class="row">
                <div v-for="message in messages">
                  <div v-if="message.user_id === partner.id">
                    <div class=".col-md-6">
                      <p>{{message.user_name}}:</p>
                      <p>{{message.text}}</p>
                      <p>{{relativeDate(message.created_at)}}</p>
                    </div>
                  </div>
                  <div v-if="message.user_id !== partner.id">
                    <div class="col-md-6 ml-auto">
                      <p>{{message.user_name}}:</p>
                      <p>{{message.text}}</p>
                      <p>{{relativeDate(message.created_at)}}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <div class="container-fluid">
              <div class="row">
                <div class="col-md-9">
                  <div class="form-group">
                    <textarea class="form-control" placeholder="Message..."></textarea> 
                  </div>
                </div>
                <div class="col-5 col-md-3">
                  <div class="form-group">
                    <button type="button" class="btn btn-primary">Send</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data: function () {
    return {
      conversations: [],
      currentConversation: {},
      messages: [],
      partner: {},
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
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
  },
};
</script>