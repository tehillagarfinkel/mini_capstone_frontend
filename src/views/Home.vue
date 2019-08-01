<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" alt="" />
      <div>
        <button v-on:click="showProduct(product)">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Description: {{ product.description }}</p>
        <p>Price: {{ product.price }}</p>
        <h4>Edit product</h4>
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
      products: [],
      currentProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    showProduct: function(product) {
      if (this.currentProduct === product) {
        this.currentProduct = {};
      } else {
        this.currentProduct = product;
      }
    },
    updateProduct: function(product) {
      var params = {
        name: product.name,
        description: product.description,
        price: product.price,
        image_url: product.image_url
      };
      axios.patch("api/products/" + product.id, params).then(response => {
        console.log("update successful", response.data);
        product.name = response.data.name;
        product.description = response.data.description;
        product.price = response.data.price;
        product.image_url = response.data.image_url;
      });
      // response = HTTP.patch("/api/products" + product.id.to_s, body: params)
      // puts "update successful", response.parse
    },
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
