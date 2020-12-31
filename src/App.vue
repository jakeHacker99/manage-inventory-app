<template>
  <div id="app">
    <AppHeader />
    <b-container>
      <b-row>
        <b-col><AddProduct @addProduct="addProduct" /></b-col>
        <b-col style="min-width: 20%"
          ><ListProduct :products="productList" @deleteProduct="deleteProduct" @updateProduct="updateProduct" 
        /></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader";
import AddProduct from "./components/AddProduct";
import ListProduct from "./components/ListProduct";
import axios from "axios";
export default {
  components: {
    AppHeader,
    AddProduct,
    ListProduct,
  },
  data() {
    return {
      productList: [],
    };
  },
  methods: {
    async getProductList() {
      try {
        let result = await axios.get("http://localhost:3000/products");
        console.log("result: ", result.data);
        this.productList = result.data;
      } catch (error) {
        console.log("Error1: ", error);
      }
    },
    async addProduct(newProduct) {
      console.log("newP", newProduct);
      try {
        await axios.post("http://localhost:3000/products", newProduct);
        this.getProductList();
      } catch (error) {
        console.log("Error2", error);
      }
    },
    async deleteProduct(productId) {
      console.log("productId: ", productId);
      try {
        await axios.delete("http://localhost:3000/products/" + productId);
        this.getProductList();
      } catch (error) {
        console.log("Error3", error);
      }
    },
    async updateProduct(updatedProduct) {
      console.log('updatedP', updatedProduct);
      try {
        await axios.put("http://localhost:3000/products/" + updatedProduct.id, updatedProduct);
        this.getProductList();
      } catch (error) {
        console.log("Error4", error);
      }
    }
  },
  mounted() {
    this.getProductList();
  },
};
</script>

<style>
.error-messsage {
  color: red;
}

#app{
  background-color:  #341d5c;
}
</style>
