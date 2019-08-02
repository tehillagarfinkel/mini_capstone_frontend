<template>
  <div class="container show">
    <img v-bind:src="product.image_url" alt="" />
    <p>Description: {{ product.description }}</p>
    <p>Price: {{ product.price }}</p>
    <router-link to="/">View all products</router-link>
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
      product: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      this.product = response.data;
      console.log(this.product);
    });
  },
  methods: {}
};
</script>
