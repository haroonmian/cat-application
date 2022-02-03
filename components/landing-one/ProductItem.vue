<template>
  <div>
    <div class="single-product-box">
      <div class="product-image">
        <div class="image-container">
          <img :src="product.image" :alt="product.name" />
        </div>
        <ul>
          <li>
            <a
              href="javascript:void(0)"
              title="Quick View"
              v-b-tooltip.hover
              @click.prevent="quickView"
            >
              <i class="far fa-eye"></i>
            </a>
          </li>
          <li>
            <a href="#" title="Add to Wishlist" v-b-tooltip.hover>
              <i class="far fa-heart"></i>
            </a>
          </li>
        </ul>
      </div>

      <div class="product-content">
        <a
          v-if="getExistPId === product.id"
          href="javascript:void(0)"
          class="btn btn-light added-btn"
          @click="addToCart(product)"
        >
          Added Already!
        </a>

        <a
          v-else
          href="javascript:void(0)"
          class="btn btn-light"
          @click="addToCart(product)"
        >
          Add to Cart
        </a>
      </div>
    </div>
    <QuickView productdetail="this.product" />
  </div>
</template>

<script>
import QuickView from "../modals/QuckView.vue";
export default {
  name: "ProductItem",
  components: {
    QuickView,
  },
  data() {
    return {
      getExistPId: null,
      productSin: {},
      product: {
        id: "",
        price: 20,
        image: "",
        name: "",
        origin: "",
      },
    };
  },
  props: {
    productSingle: Object,
    searchType: String,
  },
  computed: {
    cart() {
      return this.$store.getters.cart;
    },
  },
  beforeMount() {
    // console.log("value", this.productSingle.breeds[0].description);
    this.productSin = this.productSingle;
    this.product.id = this.productSingle.id;
    this.product.image = this.productSingle.url;
    // this.product.name = this.productSingle.breeds[0].name;
    // this.product.origin = this.productSingle.breeds[0].origin;
  },
  methods: {
    quickView(e) {
      this.$store.dispatch("detailview", this.productSin);
      this.$emit("clicked");
    },
    addToCart(item) {
      const product = [
        {
          id: item.id,
          name: item.name,
          price: 200,
          image: item.image,
          quantity: 1,
        },
      ];

      if (this.cart.length > 0) {
        let id = item.id;
        this.getExistPId = id;
        let cartIndex = this.cart.findIndex((cart) => {
          return cart.id == id;
        });

        if (cartIndex == -1) {
          this.$store.dispatch("addToCart", product);
          this.$toast("Added to cart", {
            icon: "fas fa-cart-plus",
          });
        } else {
          this.$store.dispatch("updateCart", {
            id,
            unit: 1,
            cart: this.cart,
          });
          this.$toast.info("Already added to the cart and update with one");
        }
      } else {
        this.$store.dispatch("addToCart", product);
        this.$toast("Added to cart", {
          icon: "fas fa-cart-plus",
        });
      }
    },
  },
};
</script>
