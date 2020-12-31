<template>
  <div id="app">
    <AppHeader />
    <b-container>
      <b-row class="justify-content-center">
        <AddProduct @addProduct="addProduct"/>
        <ListProduct :products="productList" @deleteProduct="deleteProduct" @updateProduct="updateProduct"/>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import AddProduct from './components/AddProduct.vue';
import ListProduct from './components/ListProduct.vue';
import axios from 'axios';

export default {
  name: 'app',
  components: {
    AppHeader,
    AddProduct,
    ListProduct
  },
  data() {
    return {
      productList: []
    }
  },
  methods: {
    async getProductList() {
      try {
        let result = await axios.get('http://localhost:3000/products');
        console.log('result', result.data);
        this.productList = result.data;
      } catch (error) {
        console.log('Error', error);
      }
    },
    async addProduct(newProduct) {
      console.log('newProduct', newProduct);
      try {
        await axios.post('http://localhost:3000/products', newProduct);
        this.getProductList();
      } catch (error) {
        console.log('Error', error);
      }
    },
    async deleteProduct(productId) {
      console.log('productId', productId);
      try {
        await axios.delete(`http://localhost:3000/products/${productId}`);
        this.getProductList();
      } catch (error) {
        console.log('Error', error);
      }
    },
    async updateProduct(updatedProduct) {
      console.log('****', updatedProduct);
      try {
        await axios.put(`http://localhost:3000/products/${updatedProduct.id}`, updatedProduct);
        this.getProductList();
      } catch (error) {
        console.log('Error', error);
      }
    }
  },
  mounted() {
    this.getProductList();
  }
}
</script>

<style>
.error-message {
  color:red;
}
</style>
