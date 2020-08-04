<template>
  <div class="users-edit">
    <div class="body-wrap">
      <section
        class="slice-lg has-bg-cover bg-size-cover"
        style="background-image: url(../../valorant-agents.jpg); background-position: top center;"
      >
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-lg-6">
              <div class="card form-card form-card--style-1">
                <div class="form-body">
                  <div class="text-center px-2">
                    <h4 class="heading heading-4 strong-400 mb-0">
                      Edit Profile
                    </h4>
                  </div>

                  <form
                    class="form-default mt-4"
                    data-toggle="validator"
                    role="form"
                    v-on:submit.prevent="editUser()"
                  >
                    <ul>
                      <li class="text-danger" v-for="error in errors">
                        {{ error }}
                      </li>
                    </ul>
                    <div class="row">
                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label">Username</label>
                          <input
                            type="text"
                            class="form-control form-control-lg"
                            v-model="user.name"
                          />
                        </div>
                      </div>

                      <div class="col-md-6">
                        <div class="form-group">
                          <label class="control-label">Email</label>
                          <input
                            type="email"
                            class="form-control form-control-lg"
                            v-model="user.email"
                          />
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-md-6">
                        <div class="form-group">
                          <label for="rankFormControlSelect">Rank</label>
                          <select
                            class="form-control"
                            id="rankFormControlSelect"
                            v-model="user.rank"
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
                          <label for="playstyleFormControlSelect"
                            >Playstyle</label
                          >
                          <select
                            class="form-control"
                            id="playstyleFormControlSelect"
                            v-model="user.playstyle"
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
                          <label class="control-label">Password</label>
                          <input
                            type="password"
                            class="form-control form-control-lg"
                            v-model="password"
                          />
                        </div>
                      </div>
                      <div class="col-md-6">
                        <div class="form-group has-feedback">
                          <label class="control-label">Confirm Password</label>
                          <input
                            type="password"
                            class="form-control form-control-lg"
                            v-model="passwordConfirmation"
                          />
                        </div>
                      </div>
                    </div>

                    <div class="row">
                      <div class="col-md-8">
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
                    <div class="row align-items-center">
                      <div class="col-6 text-left">
                        <button
                          type="submit"
                          class="btn btn-styled btn-base-1 mt-4"
                        >
                          Update
                        </button>
                      </div>
                      <div class="col-6 text-right">
                        <button
                          v-on:click="destroyUser()"
                          class="btn btn-styled btn-base-1 mt-4"
                        >
                          Delete
                        </button>
                      </div>
                    </div>
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
      user: {},
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
  created: function () {
    axios.get(`/api/users/${this.$route.params.id}`).then((response) => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {
    setFile: function (event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    editUser: function () {
      var formData = new FormData();
      formData.append("name", this.user.name);
      formData.append("email", this.user.email);
      formData.append("rank", this.user.rank);
      formData.append("playstyle", this.user.playstyle);
      if (this.user.image) {
        formData.append("image", this.user.image);
      }
      if (this.password && this.passwordConfirmation) {
        formData.append("password", this.user.password);
        formData.append("passwordConfirmation", this.user.passwordConfirmation);
      }
      axios
        .patch(`/api/users/${this.user.id}`, formData)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyUser: function () {
      if (confirm("Are you sure you want to delete your account?")) {
        axios.delete(`/api/users/${this.user.id}`).then((response) => {
          console.log("Successfully destroyed", response.data);
          this.$router.push("/");
        });
      }
    },
  },
};
</script>