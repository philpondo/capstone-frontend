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
      <!-- Top Bar -->

      <!-- Global Search -->
      <section id="sctGlobalSearch" class="global-search global-search-overlay">
        <div class="container">
          <div class="global-search-backdrop mask-dark--style-2"></div>

          <!-- Search form -->
          <form
            class="form-horizontal form-global-search z-depth-2-top"
            role="form"
          >
            <div class="px-4">
              <div class="row">
                <div class="col-12">
                  <input
                    type="text"
                    class="search-input"
                    placeholder="Type and hit enter ..."
                  />
                </div>
              </div>
            </div>
            <a
              href="#"
              class="close-search"
              data-toggle="global-search"
              title="Close search bar"
            ></a>
          </form>
        </div>
      </section>

      <!-- Navbar -->
      <nav
        class="navbar navbar-expand-lg navbar--bold navbar--bb-1px navbar-light bg-default "
      >
        <div class="container-fluid navbar-container">
          <!-- Brand/Logo -->
          <a class="navbar-brand" href="../../index.html"> </a>

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
            <!-- Navbar search - For small resolutions -->
            <div class="navbar-search-widget b-xs-bottom py-3 d-lg-none d-none">
              <form class="" role="form">
                <div class="input-group input-group-lg">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Search for..."
                  />
                  <span class="input-group-btn">
                    <button class="btn btn-base-3" type="button">
                      Go!
                    </button>
                  </span>
                </div>
              </form>
            </div>

            <!-- Navbar links -->
            <ul class="navbar-nav">
              <li class="nav-item dropdown megamenu">
                <router-link to="/" class="nav-link">
                  GoPlay
                </router-link>
              </li>
            </ul>

            <ul class="navbar-nav ml-lg-auto">
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
              <li class="nav-item">
                <router-link
                  v-if="isLoggedIn()"
                  to="/posts/new"
                  class="nav-link"
                >
                  New Post
                </router-link>
              </li>
            </ul>
          </div>

          <div class="pl-4 d-none d-lg-inline-block">
            <router-link
              v-if="!isLoggedIn()"
              to="/login"
              class="btn btn-styled btn-sm btn-base-1 text-uppercase btn-circle"
            >
              Login
            </router-link>
            <router-link
              v-if="isLoggedIn()"
              to="/logout"
              class="btn btn-styled btn-sm btn-base-1 text-uppercase btn-circle"
            >
              Logout
            </router-link>
          </div>
        </div>
      </nav>
    </div>
    <router-view />
    <footer id="footer" class="footer ">
      <div class="footer-bottom py-4">
        <div class="container">
          <div class="row cols-xs-space col-sm-space align-items-center">
            <div class="col-lg-3 col-12">
              <div class="text-center text-lg-left">
                <div class="copyright">
                  <ul class="copy-links no-margin">
                    <li>
                      &copy; 2018 Webpixels
                    </li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="col-lg-6">
              <div class="text-center">
                <ul class="footer-menu footer-menu-sm">
                  <li>
                    <a
                      href="https://webpixels.io"
                      target="_blank"
                      style="padding-left: 0;"
                      >Official website</a
                    >
                  </li>
                  <li>
                    <a
                      href="https://instagram.com/webpixels.io"
                      target="_blank"
                      style="padding-left: 0;"
                      >Instagram</a
                    >
                  </li>
                  <li>
                    <a
                      href="https://facebook.com/webpixels"
                      target="_blank"
                      style="padding-left: 0;"
                      >Facebook</a
                    >
                  </li>
                  <li>
                    <a
                      href="http://support.wrapbootstrap.com/knowledge_base/topics/usage-licenses"
                      target="_blank"
                      >License</a
                    >
                  </li>
                  <li>
                    <a href="https://goo.gl/9BbH7R" target="_blank">Buy now</a>
                  </li>
                </ul>
              </div>
            </div>

            <div class="col-lg-3">
              <div class="text-center text-lg-right">
                <ul class="social-media social-media--style-1-v4">
                  <li>
                    <a
                      href="https://www.facebook.com/webpixels/"
                      class="facebook"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Facebook"
                    >
                      <i class="fa fa-facebook"></i>
                    </a>
                  </li>
                  <li>
                    <a
                      href="https://www.instagram.com/webpixels.io/"
                      class="instagram"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Instagram"
                    >
                      <i class="fa fa-instagram"></i>
                    </a>
                  </li>
                  <li>
                    <a
                      href="https://dribbble.com/webpixels"
                      class="dribbble"
                      target="_blank"
                      data-toggle="tooltip"
                      data-original-title="Dribbble"
                    >
                      <i class="fa fa-dribbble"></i>
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
  data: function() {
    return {};
  },
  methods: {
    isLoggedIn: function() {
      return localStorage.getItem("jwt");
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    },
  },
};
</script>
