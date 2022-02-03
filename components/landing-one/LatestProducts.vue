<template>
  <div>
    <!-- pagination -->
    <div
      v-if="products.length >= 1"
      class="w-100 d-flex align-items-center justify-content-center section-title"
      style="backgroundcolor: #f5f5f5"
    >
      <b-pagination
        v-model="pageNum"
        :total-rows="rows"
        :per-page="limit"
        aria-controls="my-item"
      ></b-pagination>
    </div>
    <!-- Start All Products Area -->
    <section class="all-products-area pb-60">
      <div class="container">
        <div class="row">
          <div
            id="my-item"
            v-for="(product, index) in products"
            :key="index"
            class="col-lg-3 col-md-6 col-sm-6"
          >
            <ProductItem
              :productSingle="product"
              :searchType="searchType"
              @clicked="toggle"
            ></ProductItem>
          </div>
        </div>
      </div>
    </section>
    <!-- End all Products Area -->
    <QuckView />
    <!-- pagination -->
    <div
      v-if="products.length >= 1"
      class="w-100 d-flex align-items-center justify-content-center section-title"
      style="backgroundcolor: #f5f5f5"
    >
      <b-pagination
        v-model="pageNum"
        :total-rows="rows"
        :per-page="limit"
        aria-controls="my-item"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
import QuckView from "../modals/QuckView";
import { mutations } from "../../utils/sidebar-util";
import ProductItem from "./ProductItem";
export default {
  data() {
    return {
      limit: 12,
      products: [],
      currentPage: 1,
      rows: 80,
      searchType: "",
    };
  },
  computed: {
    pageNum: {
      set(val) {
        this.currentPage = val;
        console.log("product pagenation", val);
      },
      get() {
        return this.currentPage;
      },
    },
    currentPage: {
      set(val) {
        // fetch for best sellers
        fetch(
          `https://api.thecatapi.com/v1/images/search?breed_ids=${newVal}&limit=${this.limit}&page=${val}`
        )
          .then(async (res) => {
            this.products = [await res.json(), ...this.products];
            console.log("res", this.products);
          })
          .catch((err) => {
            console.log(err);
          });
      },
      get() {
        return this.products;
      },
    },
  },
  components: {
    QuckView,
    ProductItem,
  },
  props: {
    product: Object,
  },
  methods: {
    toggle() {
      mutations.toggleQuickView();
    },
  },
  watch: {
    "product.id": function (newVal, oldVal) {
      console.log("hello search old new....", newVal, oldVal);
      this.products = [];
      console.log(this.product);
      if (this.product.SearchingType === "breed") {
        // fetch By breed
        this.searchType = this.product.SearchingType;
        fetch(
          `https://api.thecatapi.com/v1/images/search?breed_ids=${newVal}&limit=${this.limit}&page=${this.currentPage}`
        )
          .then(async (res) => {
            this.products = await res.json();
          })
          .catch((err) => {
            console.log(err);
          });
        console.log("res", this.products);
      } else if (this.product.SearchingType === "category") {
        console.log(".......>", this.product.SearchingType, newVal);
        // fetch By category
        this.searchType = this.product.SearchingType;
        fetch(
          `https://api.thecatapi.com/v1/images/search?category_ids=${newVal}&limit=${this.limit}&page=${this.currentPage}`
        )
          .then(async (res) => {
            this.products = await res.json();
          })
          .catch((err) => {
            console.log(err);
          });
        console.log("res", this.products);
      }
    },
    currentPage: function (newpage, oldpage) {
      console.log("hello search old new....", newpage, oldpage);
      this.products = [];
      console.log(this.product);
      if (this.product.SearchingType === "breed") {
        // fetch By breed
        this.searchType = this.product.SearchingType;
        fetch(
          `https://api.thecatapi.com/v1/images/search?breed_ids=${this.product.id}&page=${newpage}&limit=${this.limit}`
        )
          .then(async (res) => {
            this.products = await res.json();
          })
          .catch((err) => {
            console.log(err);
          });
        console.log("res", this.products);
      } else if (this.product.SearchingType === "category") {
        console.log(".......>", this.product.SearchingType);
        // fetch By category
        this.searchType = this.product.SearchingType;
        fetch(
          `https://api.thecatapi.com/v1/images/search?category_ids=${this.product.id}&page=${newpage}&limit=${this.limit}`
        )
          .then(async (res) => {
            this.products = await res.json();
          })
          .catch((err) => {
            console.log(err);
          });
        console.log("res", this.products);
      }
    },
    products: function (val, oldVal) {
      console.log("VAL:", val);
      this.products = val;
    },
  },
};
</script>
