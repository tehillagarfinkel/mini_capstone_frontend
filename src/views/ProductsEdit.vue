<template>
  <div class="home">
    <h4>Edit product</h4>
    <div>
      Name:
      <input v-model="product.name" type="text" />
      Description:
      <input v-model="product.description" type="text" />
      Price:
      <input v-model="product.price" type="text" />
      Image url:
      <input v-model="product.imageUrl" type="text" />
      <button v-on:click="updateProduct(product)">Update</button>
      <button v-on:click="destroyProduct(product)">Delete</button>
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
      product: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      this.product = response.data;
      console.log(this.products);
    });
  },
  methods: {
    updateProduct: function(product) {
      var params = {
        name: product.name,
        description: product.description,
        price: product.price,
        image_url: product.image_url
      };
      axios.patch("api/products/" + product.id, params).then(response => {
        console.log("update successful", response.data);
        this.$router.push("/");
      });
    }
  }
};
</script>
