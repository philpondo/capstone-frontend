<template>
  <div class="login">
    <div>
      <form v-on:submit.prevent="submit()">
        <div>
          <h4>Login</h4>
        </div>
        <div>
          <ul>
            <li v-for="error in errors">{{ error }}</li>
          </ul>
          <div>
            <label>Email:</label>
            <input type="email" v-model="email">
          </div>
          <div>
            <label>Password:</label>
            <input type="password" v-model="password">
          </div>
        </div>
        <div>
          <input type="submit" value="Login">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        email: this.email,
        password: this.password,
      };
      axios
        .post("/api/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          this.$router.push("/");
        })
        .catch((error) => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>