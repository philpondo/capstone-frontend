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
                    <select class="form-control" v-model="rank">
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
                  <div class="form-group">
                    <label>Playstyle:</label> 
                    <select class="form-control" v-model="playstyle">
                      <option>beginner</option>
                      <option>casual</option>
                      <option>competitive</option>
                    </select>
                  </div>
                  <div class="form-group">
                    <label>Password:</label>
                    <input type="password" class="form-control" v-model="password">
                  </div>
                  <div class="form-group">
                    <label>Confirm Password:</label>
                    <input type="password" class="form-control" v-model="passwordConfirmation">
                  </div>
                  <div class="form-group">
                    <label>Upload a Profile Picture: </label>
                    <input type="file" v-on:change="setFile($event)" ref="fileInput">
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