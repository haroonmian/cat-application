<template>
  <div>
    <!-- Start Page Title Area -->
    <div class="page-title-area">
      <div class="container">
        <ul>
          <li><nuxt-link to="/">Home</nuxt-link></li>
          <li>All Products</li>
        </ul>
      </div>
    </div>
    <!-- End Page Title Area -->

    <!-- Start Collections Area -->
    <section class="products-collections-area ptb-60">
      <div class="container">
        <div class="section-title">
          <h2><span class="dot"></span> Products</h2>
        </div>

        <div class="row">
          <!-- <Sidebar /> -->
        </div>
      </div>
    </section>
    <!-- End Collections Area -->
  </div>
</template>

<script>
// import Sidebar from "../components/all-products/Sidebar";
export default {
  components: {
    // Sidebar,
  },
};
</script>
