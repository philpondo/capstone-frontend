<template>
  <div class="signup">
    <!-- <div>
      <form v-on:submit.prevent="submit()">
        <div>
          <h4>Signup</h4>
        </div>
        <div>
          <ul>
            <li class="text-danger" v-for="error in errors">{{ error }}</li>
          </ul>
          <div>
            <label>Name:</label> 
            <input type="text" v-model="name">
          </div>
          <div>
            <label>Email:</label>
            <input type="email" v-model="email">
          </div>
          <div>
            <label>Rank:</label> 
            <select v-model="rank">
              <option>Unranked</option>
              <option>Iron I</option>
              <option>Iron II</option>
              <option>Iron III</option>
              <option>Bronze I</option>
              <option>Bronze II</option>
              <option>Bronze III</option>
              <option>Silver I</option>
              <option>Silver II</option>
              <option>Silver III</option>
              <option>Gold I</option>
              <option>Gold II</option>
              <option>Gold III</option>
              <option>Platinum I</option>
              <option>Platinum II</option>
              <option>Platinum III</option>
              <option>Diamond I</option>
              <option>Diamond II</option>
              <option>Diamond III</option>
              <option>Immortal I</option>
              <option>Immortal II</option>
              <option>Immortal III</option>
              <option>Radiant</option>
            </select>
          </div>
          <div>
            <label>Playstyle:</label> 
            <select v-model="playstyle">
              <option>beginner</option>
              <option>casual</option>
              <option>competitive</option>
            </select>
          </div>
          <div>
            <label>Password:</label>
            <input type="password" v-model="password">
          </div>
          <div>
            <label>Confirm Password:</label>
            <input type="password" v-model="passwordConfirmation">
          </div>
          <div>
            <label>Upload a Profile Picture: </label>
            <input type="file" v-on:change="setFile($event)" ref="fileInput">
          </div>
        </div>
        <div>
          <input type="submit" value="Signup">
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
            <div class="col-lg-6">
              <div class="card bg-dark border-dark form-card form-card--style-1">
                <div class="form-body">
                  <div class="text-center px-2">
                    <h4 class="heading heading-4 strong-400 mb-0 text-white">
                      Create a new account
                    </h4>
                  </div>

                  <form
                    class="form-default mt-4"
                    data-toggle="validator"
                    role="form"
                    v-on:submit.prevent="submit()"
                  >
                    <ul>
                      <li class="text-danger" v-for="error in errors">
                        {{ error }}
                      </li>
                    </ul>
                    <div class="row">
                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label text-white">Username</label>
                          <input
                            type="text"
                            class="form-control form-control-lg"
                            v-model="name"
                          />
                        </div>
                      </div>

                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label text-white">Email</label>
                          <input
                            type="email"
                            class="form-control form-control-lg"
                            v-model="email"
                          />
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label text-white" for="rankFormControlSelect">Rank</label>
                          <select
                            class="form-control"
                            id="rankFormControlSelect"
                            v-model="rank"
                          >
                            <option>Unranked</option>
                            <option>Iron I</option>
                            <option>Iron II</option>
                            <option>Iron III</option>
                            <option>Bronze I</option>
                            <option>Bronze II</option>
                            <option>Bronze III</option>
                            <option>Silver I</option>
                            <option>Silver II</option>
                            <option>Silver III</option>
                            <option>Gold I</option>
                            <option>Gold II</option>
                            <option>Gold III</option>
                            <option>Platinum I</option>
                            <option>Platinum II</option>
                            <option>Platinum III</option>
                            <option>Diamond I</option>
                            <option>Diamond II</option>
                            <option>Diamond III</option>
                            <option>Immortal I</option>
                            <option>Immortal II</option>
                            <option>Immortal III</option>
                            <option>Radiant</option>
                          </select>
                        </div>
                      </div>
                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label text-white" for="playstyleFormControlSelect"
                            >Playstyle</label
                          >
                          <select
                            class="form-control"
                            id="playstyleFormControlSelect"
                            v-model="playstyle"
                          >
                            <option>beginner</option>
                            <option>casual</option>
                            <option>competitive</option>
                          </select>
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label text-white">Password</label>
                          <input
                            type="password"
                            class="form-control form-control-lg"
                            v-model="password"
                          />
                        </div>
                      </div>
                      <div class="col-md-6">
                        <div class="form-group has-feedback">
                          <label class="control-label text-white">Confirm Password</label>
                          <input
                            type="password"
                            class="form-control form-control-lg"
                            v-model="passwordConfirmation"
                          />
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-md-9">
                        <input
                          type="file"
                          name="file-1[]"
                          id="file-1"
                          class="custom-input-file custom-input-file--1"
                          data-multiple-caption="{count} files selected"
                          multiple
                          v-on:change="setFile($event)"
                          ref="fileInput"
                        />
                        <label for="file-1">
                          <i class="fa fa-upload"></i>
                          <span>Upload a Profile Picture</span>
                        </label>
                      </div>
                    </div>

                    <button
                      type="submit"
                      class="btn btn-styled bg-red mt-4"
                    >
                      Sign up
                    </button>
                  </form>
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
  data: function () {
    return {
      name: "",
      email: "",
      password: "",
      passwordConfirmation: "",
      rank: "",
      playstyle: "",
      image: "",
      errors: [],
    };
  },
  methods: {
    setFile: function (event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    submit: function () {
      var formData = new FormData();
      formData.append("name", this.name);
      formData.append("email", this.email);
      formData.append("password", this.password);
      formData.append("passwordConfirmation", this.passwordConfirmation);
      formData.append("rank", this.rank);
      formData.append("playstyle", this.playstyle);
      formData.append("image", this.image);
      axios
        .post("/api/users", formData)
        .then((response) => {
          this.$router.push("/login");
          this.name = "";
          this.email = "";
          this.password = "";
          this.passwordConfirmation = "";
          this.rank = "";
          this.playstyle = "";
          this.$refs.fileInput.value = "";
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
