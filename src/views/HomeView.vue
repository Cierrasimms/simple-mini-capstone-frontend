<template>
  <div class="home">
    <h1>{{ message }} count: {{ products.length }}</h1>
    <h2>New Product</h2>
    Name
    <input type="text" v-model="newProduct.name" />
    Description
    <input type="text" v-model="newProduct.description" />
    Image Url
    <input type="text" v-model="newProduct.image_url" />
    Price
    <input type="text" v-model="newProduct.price" />

    <button v-on:click="createProduct()">Create</button>

    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" style="max-width: 250px" />
      <p>{{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.title }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <h1>Edit Product</h1>
        <p>
          Name
          <input v-model="currentProduct.name" type="text" />
        </p>
        <p>
          Description
          <input v-model="currentProduct.description" type="text" />
        </p>
        <p>
          Price
          <input v-model="currentProduct.price" type="text" />
        </p>
        <button v-on:click="updateProduct()">Update</button>
        <button v-on:click="deleteProduct(currentProduct)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Buy my things pls",
      products: [],
      newProduct: {},
      currentProduct: {},
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
      axios
        .post("http://localhost:3000/products", this.newProduct)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    showProduct(product) {
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct() {
      axios.patch(`http://localhost:3000/products/${this.currentProduct.id}`, this.currentProduct).then((response) => {
        console.log("Success!", response);
      });
    },
    deleteProduct(product) {
      axios.delete(`http://localhost:3000/products/${product.id}`).then((response) => {
        console.log(response);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
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
