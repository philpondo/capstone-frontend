<template>
  <div id="app">
    <!-- <div class="header">
      <nav class="navbar navbar-expand-lg navbar--bold  navbar-transparent navbar-inverse bg-dark">
        <div class="container navbar-container">
          <router-link to="/">/ Home /</router-link> 
          <router-link v-if="isLoggedIn()" to="/posts/new">/ New Post /</router-link> 
          <router-link v-if="isLoggedIn()" v-bind:to="`/users/${getUserId()}`">/ My Profile /</router-link> 
          <router-link v-if="isLoggedIn()" to="/conversations">/ My Conversations /</router-link> 
          <router-link v-if="isLoggedIn()" to="/logout">/ Logout /</router-link>
          <router-link v-if="!isLoggedIn()" to="/login">/ Login /</router-link> 
          <router-link v-if="!isLoggedIn()" to="/signup">/ Signup /</router-link>
        </div>
      </nav>
    </div> -->
    <!-- HEADER -->
    <div class="header">
      <!-- Navbar -->
      <nav
        class="navbar navbar-expand-lg navbar--bold navbar--bb-1px navbar-inverse bg-dark"
      >
        <div class="container-fluid navbar-container">
          <div class="d-inline-block">
            <!-- Navbar toggler  -->
            <button
              class="navbar-toggler hamburger hamburger-js hamburger--spring"
              type="button"
              data-toggle="collapse"
              data-target="#navbar_main"
              aria-controls="navbarsExampleDefault"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              <span class="hamburger-box">
                <span class="hamburger-inner"></span>
              </span>
            </button>
          </div>

          <div
            class="collapse navbar-collapse align-items-center justify-content-around"
            id="navbar_main"
          >
            <!-- Navbar links -->
            <ul class="navbar-nav">
              <li class="nav-item dropdown megamenu">
                <router-link class="nav-link" to="/">
                  GoPlay
                </router-link>
              </li>
            </ul>

            <ul class="navbar-nav ml-lg-auto">
              <li class="nav-item">
                <router-link
                  v-if="isLoggedIn()"
                  to="/posts/new"
                  class="nav-link"
                >
                  New Post
                </router-link>
              </li>
              <li class="nav-item">
                <router-link
                  v-if="isLoggedIn()"
                  v-bind:to="`/users/${getUserId()}`"
                  class="nav-link"
                >
                  My Profile
                </router-link>
              </li>
              <li class="nav-item">
                <router-link
                  v-if="isLoggedIn()"
                  to="/conversations"
                  class="nav-link"
                >
                  My Conversations
                </router-link>
              </li>
            </ul>
          </div>

          <div class="pl-4 d-none d-lg-inline-block">
            <router-link
              v-if="!isLoggedIn()"
              to="/login"
              class="btn btn-styled btn-sm text-uppercase btn-circle bg-red"
            >
              Login
            </router-link>
            <router-link
              v-if="isLoggedIn()"
              to="/logout"
              class="btn btn-styled btn-sm text-uppercase btn-circle bg-red"
            >
              Logout
            </router-link>
          </div>
        </div>
      </nav>
    </div>
    <router-view />
    <footer id="footer" class="footer">
      <div class="footer-bottom py-4 bg-dark">
        <div class="container">
          <div class="row cols-xs-space col-sm-space align-items-center">
            <div class="col-lg-3 col-12">
              <div class="text-center text-lg-left">
                <div class="copyright">
                  <ul class="copy-links no-margin">
                    <li class="text-white">Phil Pondo 2020</li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="col-lg-6">
              <div class="text-center">
                <ul class="footer-menu footer-menu-sm">
                  <li>
                    <a
                      href="https://philpondo.github.io"
                      target="_blank"
                      style="padding-left: 0;"
                      >Personal website</a
                    >
                  </li>
                  <li>
                    <a
                      href="https://github.com/philpondo"
                      target="_blank"
                      style="padding-left: 0;"
                      >Github</a
                    >
                  </li>
                  <li>
                    <a
                      href="https://linkedin.com/in/phillippondo"
                      target="_blank"
                      style="padding-left: 0;"
                      >LinkedIn</a
                    >
                  </li>
                  <li>
                    <a
                      href="http://twitter.com/philpondo"
                      target="_blank"
                      >Twitter</a
                    >
                  </li>
                </ul>
              </div>
            </div>

            <div class="col-lg-3">
              <div class="text-center text-lg-right">
                <ul class="social-media social-media--style-1-v4">
                  <li>
                    <a
                      href="https://github.com/philpondo"
                      class="github"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Github"
                    >
                      <i class="fa fa-github"></i>
                    </a>
                  </li>
                  <li>
                    <a
                      href="https://linkedin.com/in/phillippondo"
                      class="linkedin"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Linkedin"
                    >
                      <i class="fa fa-linkedin"></i>
                    </a>
                  </li>
                  <li>
                    <a
                      href="http://twitter.com/philpondo"
                      class="twitter"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Tribbble"
                    >
                      <i class="fa fa-twitter"></i>
                    </a>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style></style>
<script>
export default {
  data: function () {
    return {};
  },
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
    getUserId: function () {
      return localStorage.getItem("user_id");
    },
  },
};
</script>
