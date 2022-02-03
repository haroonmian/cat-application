<template>
  <client-only>
    <!-- Start Partner Area -->
    <div class="partner-area">
      <div class="container">
        <div class="partner-slides owl-carousel owl-theme">
          <div class="w-100">
            <h4>Search By Options:</h4>
          </div>
          <div
            class="w-100 d-flex align-items-center justify-content-space-evenly, flex-wrap"
          >
            <div class="w-50 pr-4">
              <!-- Selecter -->
              <div v-if="SearchType === 'breed'">
                <b-form-select v-model="selected" :options="breeds"></b-form-select>
              </div>

              <!-- category -->
              <div
                v-if="SearchType === 'category'"
                class="w-100 d-flex align-items-center justify-content-space-evenly, flex-wrap"
              >
                <b-form-select v-model="selected" :options="categories"></b-form-select>
              </div>
            </div>
            <!-- Search Type -->
            <div
              class="w-50 d-flex align-items-center flex-wrap"
              style="justify-content: end"
            >
              <b-form-radio
                name="radio-size"
                size="sm"
                v-model="SearchType"
                value="category"
                >search by category&emsp;</b-form-radio
              >

              <b-form-radio name="radio-size" size="sm" v-model="SearchType" value="breed"
                >search by breed</b-form-radio
              >
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Partner Area -->
  </client-only>
</template>
<script>
export default {
  name: "PartnerComponent",
  props: {
    productByCategory: Function,
  },
  data() {
    return {
      categories: [],
      breeds: [],
      SearchType: "category",
      s: null,
    };
  },
  computed: {
    selected: {
      set(val) {
        this.s = val;
        this.productByCategory(val, this.SearchType);
      },
      get() {
        return this.s;
      },
    },
  },
  async mounted() {
    await fetch("https://api.thecatapi.com/v1/breeds")
      .then(async (res) => {
        const resData = await res.json();
        this.breeds = [{ value: resData.id, text: resData.name }];
        resData.map((a, index) => {
          this.breeds[index] = { value: a.id, text: a.name };
          a.name;
        });
        // console.log("categories", this.categories);
      })
      .catch((error) => console.log("error", error));
    await fetch("https://api.thecatapi.com/v1/categories")
      .then(async (res) => {
        const resData = await res.json();
        this.categories = [{ value: resData.id, text: resData.name }];
        // this.categories = resData;
        resData.map((a, index) => {
          this.categories[index] = { value: a.id, text: a.name };
          a.name;
        });
        console.log("categories", this.categories);
      })
      .catch((error) => console.log("error", error));
  },
};
</script>
