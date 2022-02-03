<template>
  <div>
    <Banner></Banner>
    <Partner :productByCategory="productByCategory"></Partner>
    <LatestProducts :product="product"></LatestProducts>
    <BestSellers v-if="!product.id"></BestSellers>
    <Testimonials></Testimonials>
  </div>
</template>

<script>
import Banner from "../components/landing-one/Banner";
import LatestProducts from "../components/landing-one/LatestProducts";
import BestSellers from "../components/landing-one/BestSellers";
import Testimonials from "../components/landing-one/Testimonials";
import Partner from "../components/common/Partner.vue";
import axios from "axios";

export default {
  data() {
    return {
      product: {
        id: null,
        SearchingType: null,
      },
    };
  },

  beforeCreate() {
    axios
      .get("https://api.thecatapi.com/v1/images/search?limit=9&order=Asc&page=0")
      .then((result) => {
        console.log("result before created:", result);
      });
  },

  components: {
    Banner,
    LatestProducts,
    BestSellers,
    Testimonials,
    Partner,
  },

  methods: {
    productByCategory(id, SearchType) {
      this.product.id = id;
      this.product.SearchingType = SearchType;
    },
  },
};
</script>
