<template>
  <div class="home">
    <h1>New Product</h1>
    Name:
    <input v-model="name" type="text" />
    Description:
    <input v-model="description" type="text" />
    Price:
    <input v-model="price" type="text" />
    Supplier id:
    <input v-model="supplierId" type="text" />
    Image url:
    <input v-model="imageUrl" type="text" />
    <button v-on:click="createProduct()">Create</button>
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" alt="" />
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
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
      name: "",
      description: "",
      price: "",
      supplierId: "",
      imageUrl: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    createProduct() {
      var params = {
        name: this.name,
        description: this.description,
        price: this.price,
        supplier_id: this.supplierId,
        image_url: this.imageUrl
      };
      axios
        .post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.products.push(response.data);
          this.name = "";
          this.description = "";
          this.price = "";
          this.supplierId = "";
          this.imageUrl = "";
        })
        .catch(error => console.log(error.response));
    }
  }
};
</script>
