<template>
  <div>
    <!-- Start Page Title Area -->
    <div class="page-title-area">
      <div class="container">
        <ul>
          <li><nuxt-link to="/">Home</nuxt-link></li>
          <li>{{ product.name }}</li>
        </ul>
      </div>
    </div>
    <!-- End Page Title Area -->

    <!-- Start Products Details Area -->
    <section class="products-details-area ptb-60">
      <div class="container">
        <div class="row">
          <Details
            :id="product.id"
            :name="product.name"
            :price="product.price"
            :image="product.image"
          />
          <DetailsInfo />
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Details from "../../components/products/Details";
import DetailsInfo from "../../components/products/DetailsInfo";
export default {
  components: {
    Details,
    DetailsInfo,
  },
  data() {
    return {
      id: this.$route.params.id,
    };
  },
  computed: {
    product() {
      return this.$store.state.products.all.find(
        (product) => product.id === parseInt(this.id)
      );
    },
  },
};
</script>
