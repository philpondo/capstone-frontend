<template>
  <div class="conversations-index">
    <div v-if="conversations.length == 0">
      <h5>No Conversations</h5>
    </div>
    <div v-for="conversation in orderBy(conversations, 'created_at', -1)">
      <div v-if="conversation.last_message">
        <h5>Conversation with {{conversation.partner.name}}</h5>
        <p v-if="conversation.last_message">Last Message: {{conversation.last_message.text}}</p>
        <p>{{relativeDate(conversation.last_message.created_at)}}</p>
        <!-- Button trigger modal -->
        <button v-on:click="showConversation(conversation)" type="button">
          Open
        </button>
        <button v-on:click="deleteConversation(conversation)" type="button">
          Delete
        </button>
      </div>
    </div>
    <div v-if="currentConversation">
      <h5>Conversation with {{partner.name}}</h5>
      <div>
        <div v-for="message in messages">
          <p>{{message.user_name}}:</p>
          <p>{{message.text}}</p>
          <p>{{relativeDate(message.created_at)}}</p>
        </div>
      </div>
      <div>
        <div>
          <textarea v-model="text" placeholder="Message..."></textarea> 
        </div>
      </div>
      <div>
        <button type="button" v-on:click="createMessage()">Send</button>
        <button type="button" v-on:click="currentConversation = ''">Close</button>
      </div>
    </div>
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