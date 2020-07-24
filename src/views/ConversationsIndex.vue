<template>
  <div class="conversations-index">
    <div v-for="conversation in conversations">
      <h3>Conversation with {{conversation.partner.name}}</h3>
      <p>Last Message: {{conversation.last_message.text}}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      conversations: [],
    };
  },
  created: function () {
    axios.get("/api/conversations").then((response) => {
      console.log("All Conversations:", response.data);
      this.conversations = response.data;
    });
  },
  methods: {},
};
</script>