<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
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

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js",
      products: []
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
