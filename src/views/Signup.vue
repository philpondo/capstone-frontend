<template>
  <div class="signup">
    <div class="container">
      <div class="row">
        <div class="col">
          <div class="card text-center text-white bg-dark mb-3">
            <form v-on:submit.prevent="submit()">
              <div class="card-header">
                <h4>Signup</h4>
              </div>
              <div class="card-body">
                <div class="container">
                  <ul>
                    <li class="text-danger" v-for="error in errors">{{ error }}</li>
                  </ul>
                  <div class="form-group">
                    <label>Name:</label> 
                    <input type="text" class="form-control" v-model="name">
                  </div>
                  <div class="form-group">
                    <label>Email:</label>
                    <input type="email" class="form-control" v-model="email">
                  </div>
                  <div class="form-group">
                    <label>Rank:</label> 
                    <input type="text" class="form-control" v-model="rank">
                  </div>
                  <div class="form-group">
                    <label>Playstyle:</label> 
                    <input type="text" class="form-control" v-model="playstyle">
                  </div>
                  <div class="form-group">
                    <label>Password:</label>
                    <input type="password" class="form-control" v-model="password">
                  </div>
                  <div class="form-group">
                    <label>Confirm Password:</label>
                    <input type="password" class="form-control" v-model="passwordConfirmation">
                  </div>
                </div>
              </div>
              <div class="card-footer">
                <input type="submit" class="btn btn-primary" value="Signup">
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      name: "",
      email: "",
      password: "",
      passwordConfirmation: "",
      rank: "",
      playstyle: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        name: this.name,
        email: this.email,
        password: this.password,
        password_confirmation: this.passwordConfirmation,
        rank: this.rank,
        playstyle: this.playstyle,
      };
      axios
        .post("/api/users", params)
        .then((response) => {
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>