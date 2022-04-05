<template>
  <div class="home">
    <h1>{{ message }} count: {{ products.length }}</h1>
    <h2>New Product</h2>
    Name
    <input type="text" v-model="productName" />
    Description
    <input type="text" v-model="productDescription" />
    Image Url
    <input type="text" v-model="productImageUrl" />
    Price
    <input type="text" v-model="productPrice" />

    <button v-on:click="createProduct()">Create</button>
    "
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      <p>{{ product.description }}</p>

      <p>Price: {{ product.price }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Buy my things pls",
      products: [],
      productName: "",
      productDescription: "",
      productImageUrl: "",
      productPrice: 0,
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct() {
      var params = {
        name: this.productName,
        description: this.productDescription,
        image_url: this.productImageUrl,
        price: this.productPrice,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<style>
h1 {
  color: rgba(140, 0, 255, 0.745);
  font-size: 45px;
}
h2 {
  color: rgb(91, 179, 175);
  font-size: 45px;
}
</style>
