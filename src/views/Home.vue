<template>
  <div class="home">
    <div>
      <button v-on:click="sortAttribute = 'name'" class="btn btn-primary">Sort by Product</button>
      <button v-on:click="sortAttribute = 'price'" class="btn btn-primary">Sort by Price</button>
    </div>
    Search:
    <input v-model="searchFilter" type="text" list="names" />
    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>
    <div v-for="product in orderBy(filterBy(products, searchFilter, 'name'), sortAttribute)">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" alt="" />
      <router-link v-bind:to="`/products/${product.id}`">More Info</router-link>
    </div>
  </div>
</template>

<style>
img {
  width: 100%;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      products: [],
      searchFilter: "",
      sortAttribute: "name"
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    destroyProduct: function(product) {
      axios.delete("api/products/" + product.id).then(response => {
        console.log("delete successful", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
