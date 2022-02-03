<template>
  <div>
    <div class="modal-backdrop" style="margintop: 3rem" v-if="isQuickViewOpen"></div>
    <transition name="slide-fade">
      <!-- Start Products QuickView Modal Area -->
      <div
        v-if="isQuickViewOpen"
        class="modal fade productQuickView"
        id="productQuickView"
        tabindex="-1"
        role="dialog"
        aria-hidden="true"
      >
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <button type="button" class="close" @click="closeQuickView">
              <span aria-hidden="true"><i class="fas fa-times"></i></span>
            </button>
            <div class="row align-items-center">
              <div class="col-lg-6 col-md-6">
                <div class="productQuickView-image">
                  <img :src="_itemImg" alt="image note found " />
                </div>
              </div>
              <div></div>
              <div class="col-lg-6 col-md-6">
                <div class="product-content">
                  <h3>{{ item.name }}</h3>

                  <div class="price">
                    <span class="new-price">$ &nbsp; {{ item.price }}</span>
                  </div>
                  <ul class="product-info">
                    <li>
                      <span>Availability In stock</span>
                    </li>
                  </ul>
                  <br />
                  <p>{{ item.description }}</p>
                  <br />
                  <div class="product-add-to-cart">
                    <div class="input-counter">
                      <span @click="decreaseQuantity()" class="minus-btn"
                        ><i class="fas fa-minus"></i
                      ></span>
                      {{ quantity }}
                      <span @click="increaseQuantity()" class="plus-btn"
                        ><i class="fas fa-plus"></i
                      ></span>
                    </div>

                    <button
                      type="submit"
                      class="btn btn-primary"
                      @click="addToCart(item)"
                    >
                      <i class="fas fa-cart-plus"></i> Add to Cart
                    </button>
                  </div>
                  <h3
                    class="view-full-info"
                    @click="closeQuickView"
                    style="cursor: pointer"
                  >
                    &nbsp;&nbsp;&nbsp;close
                  </h3>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- End Products QuickView Modal Area -->
    </transition>
  </div>
</template>

<script>
import { store, mutations } from "../../utils/sidebar-util";
export default {
  name: "QuckView",
  data() {
    return {
      quantity: 1,
      item: {
        id: null,
        name: "",
        price: 200.0,
        bestSellers: false,
        trending: false,
        origin: "",
        image: null,
        description: "",
        timePeriod: false,
        dateTime: new Date("December 19, 2020 17:00:00 PDT"),
      },
    };
  },
  updated() {
    console.log("getters,,......", this.$store.state.detailview.name);
    const { id, url, breeds } = this.$store.state.detailview;
    this.item.id = id;
    this._itemImg = url;
    this.item.origin = origin ? origin : "";
    if (breeds.length >= 1) {
      this.item.name = breeds[0].name ? breeds[0].name : "";
      this.item.description = breeds[0].description ? breeds[0].description : "";
      console.log("test vlaue///////////", breeds[0].description);
    }
  },
  watch: {
    _itemImg: function (val, oldVal) {
      console.log("val: ", val);
      this.item.image = val;
    },
  },
  methods: {
    closeQuickView: mutations.toggleQuickView,
    addToCart(item) {
      const product = [
        {
          id: item.id,
          name: item.name,
          price: item.price,
          image: item.image,
          quantity: this.quantity,
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

      this.closeQuickView();
    },
    increaseQuantity() {
      if (this.quantity > 9) {
        this.$toast.error("Can't add more than 10", {
          icon: "fas fa-cart-plus",
        });
      } else {
        this.quantity++;
      }
    },
    decreaseQuantity() {
      if (this.quantity < 2) {
        this.$toast.error("Can't add less than 1", {
          icon: "fas fa-cart-plus",
        });
      } else {
        this.quantity--;
      }
    },
  },
  computed: {
    _itemImg: {
      set(val) {
        this.item.image = val;
      },
      get() {
        return this.item.image;
      },
    },
    isQuickViewOpen() {
      return store.isQuickViewOpen;
    },
    cart() {
      return this.$store.getters.cart;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../../assets/scss/styles/_transitions.scss";
</style>
