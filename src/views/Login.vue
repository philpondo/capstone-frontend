<template>
  <div class="login">
    <!-- <div>
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
            <input type="email" v-model="email" />
          </div>
          <div>
            <label>Password:</label>
            <input type="password" v-model="password" />
          </div>
        </div>
        <div>
          <input type="submit" value="Login" />
        </div>
      </form>
    </div> -->

    <div class="body-wrap">
      <section
        class="slice-lg has-bg-cover bg-size-cover"
        style="background-image: url(../../valorant-agents.jpg); background-position: top center;"
      >
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-lg-5">
              <div class="card form-card form-card--style-2">
                <div class="form-header text-center">
                  <div class="form-header-icon">
                    <i class="icon ion-log-in"></i>
                  </div>
                </div>
                <div class="form-body">
                  <div class="text-center px-2">
                    <h4 class="heading heading-4 strong-4">
                      Sign in to find teammates!
                    </h4>
                  </div>

                  <form
                    class="form-default"
                    role="form"
                    v-on:submit.prevent="submit()"
                  >
                    <ul>
                      <li v-for="error in errors">{{ error }}</li>
                    </ul>
                    <div class="row">
                      <div class="col-12">
                        <div class="form-group">
                          <label>Email</label>
                          <input
                            type="email"
                            class="form-control form-control-lg"
                            v-model="email"
                          />
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-lg-12">
                        <div class="form-group has-feedback">
                          <label>Password</label>
                          <input
                            type="password"
                            class="form-control form-control-lg"
                            v-model="password"
                          />
                        </div>
                      </div>
                    </div>

                    <button
                      type="submit"
                      class="btn btn-styled btn-lg btn-base-1 btn-block mt-4"
                    >
                      Sign in
                    </button>
                  </form>
                </div>
              </div>

              <!-- Form auxiliary links -->
              <div class="form-user-footer-links pt-2">
                <div class="row">
                  <div class="col-12 text-center">
                    <router-link to="/signup" class="text-white"
                      >Don't have an account? Sign up here</router-link
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function() {
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
