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
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      name: "",
      description: "",
      price: "",
      supplierId: "",
      imageUrl: ""
    };
  },

  methods: {
    createProduct: function() {
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
          this.$router.push("/");
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
