<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: {},
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log("All recipes:", response.data);
      });
    },
    createProduct: function () {
      console.log("Create a product...");

      var params = this.newProductParams;

      axios
        .post("http://localhost:3000/products.json", params)
        .then((response) => {
          console.log("Success", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showProduct: function (product) {
      console.log("show product", product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      console.log("Updating product...", product);
      axios.patch("http://localhost:3000/products/" + product.id + ".json", product).then((response) => {
        console.log("Successful", response.data);
      });
    },
    destroyProduct: function (product) {
      console.log("Destroying product...");
      axios.delete("http://localhost:3000/products/" + product.id + ".json").then((response) => {
        console.log("Successfully destroyed product", response.data);
      });
      var index = this.products.indexOf(product);
      this.products.splice(index, 1);
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
    </div>
    <div>
      Price:
      <input type="text" v-model="newProductParams.price" />
    </div>
    <div>
      Description:
      <input type="text" v-model="newProductParams.description" />
    </div>
    <div>
      Image URL:
      <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" />
      <p>Price: {{ product.price }}</p>
      <button v-on:click="showProduct(product)">Show more info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Product: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Tax: {{ currentProduct.tax }}</p>
        <p>Total: {{ currentProduct.total }}</p>
        <h1>Edit Product</h1>
        <p>
          Product:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>
          Tax:
          <input type="text" v-model="currentProduct.tax" />
        </p>
        <p>
          Total:
          <input type="text" v-model="currentProduct.total" />
        </p>
        <p>
          <button v-on:click="updateProduct(currentProduct)">Update product</button>
        </p>
        <p>
          <button v-on:click="destroyProduct(currentProduct)">Delete product</button>
        </p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
