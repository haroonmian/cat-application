<template>
  <footer class="footer-area">
    <div class="container">
      <div class="row">
        <div class="col-lg-3 col-md-6 col-sm-6">
          <div class="single-footer-widget">
            <div class="logo">
              <nuxt-link to="/">
                <!-- <img src="../assets/img/logo.png" alt="logo" /> -->
              </nuxt-link>
            </div>

            <p>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
              tempor incididunt ut labore et dolore magna aliqua. Quis ipsum suspendisse
              ultrices gravida.
            </p>
          </div>
        </div>

        <div class="col-lg-3 col-md-6 col-sm-6">
          <div class="single-footer-widget">
            <h3>Quick Links</h3>

            <ul class="quick-links">
              <li><nuxt-link to="/">Home</nuxt-link></li>
              <li><nuxt-link to="/cart">Cart</nuxt-link></li>
              <li><nuxt-link to="/">Contact Us</nuxt-link></li>
            </ul>
          </div>
        </div>

        <div class="col-lg-3 col-md-6 col-sm-6">
          <div class="single-footer-widget">
            <h3>Information</h3>

            <ul class="information-links">
              <li><nuxt-link to="/components">Login</nuxt-link></li>
              <li><nuxt-link to="/">Signup</nuxt-link></li>
              <li><nuxt-link to="/">Contact Us</nuxt-link></li>
            </ul>
          </div>
        </div>

        <div class="col-lg-3 col-md-6 col-sm-6">
          <div class="single-footer-widget">
            <h3>Contact Us</h3>
          </div>
        </div>
      </div>
    </div>

    <div class="copyright-area">
      <div class="container">
        <div class="row align-items-center"></div>
      </div>
    </div>
  </footer>
</template>
