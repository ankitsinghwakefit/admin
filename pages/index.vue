<template>
<main>
  <!-- space bar below -->
  <div class="a-spacing-large"></div>
  <div class="container-fluid browsing-history">
    <div class="row">
      <div class="col-sm-8 col-8">
        <h1 class="a-size-large a-spacing-none a-text-normal">
        Hello Admin, You can add products here for ecommerce website.
        </h1>
        <h1 class="a-size-large a-spacing-none a-text-normal">
        Below are the All listed Products.
        </h1>
          <div class="a-spacing-large"></div>
          <nuxt-link to="/products" class="a-button-buy-again">
            Add a new Product
          </nuxt-link>
          <!-- <a href="#" class="a-button-history margin-right-10">
            Add a new Category
          </a>
          <a href="#" class="a-button-history margin-right-10">
            Add a new Owner
          </a> -->
      </div>
    </div>
  </div>
<div class="a-spacing-large"></div>
  <div class="container-fluid browsing-history">
    <div class="row">
      <div v-for="(product, index) in products"
      :key="product._id"
      class="col-xl-2 col-lg-2 col-md-3 col-sm-6 col-6 br bb bl bt ml-1">
      <div class="history-box">
        <!-- product image -->
        <a href="#" class="a-link-normal">
          <img :src="product.photo" class="img-fluid">
        </a>
        <!-- product title -->
        <div class="a-spacing-top-base asin-title">
          <span class="a-text-normal">
            <div class="pl3n-sc-truncated">Product Name: {{product.title}}</div>
          </span>
        </div>
        <!-- product rating -->
        <div class="a-row">
          <a href="#">
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
          </a>
          <span class="a-letter-space"></span>
          <span class="a-color-teritary a-size-small asin-reviews">(1732)</span>
        </div>
          <!-- product priceing -->
        <div class="a-row">
          <span class="a-size-base a-color-price">
            <span class="p13n-sc-price">Rs. {{product.price}}/-</span>
          </span>
        </div>
        <!-- product description -->
        <div class="a-row">
          <span class="a-size-base a-text-normal">
            <span class="p13n-sc-price">Description: {{product.description}}</span>
          </span>
        </div>
        <!-- product stock quantity -->
        <div class="a-row">
          <span class="a-size-base a-text-normal">
            <span class="p13n-sc-price">Stock Quantity: {{product.stockQuantity}}</span>
          </span>
        </div>
        <!-- product buttons -->
        <div class="a-row">
          <nuxt-link :to="`/products/${product._id}`" class="a-button-history margin-right-10">Update</nuxt-link>
          <a @click="onDeleteProduct(product._id, index)" class="a-button-history margin-right-10">Delete</a>
        </div>
      </div>
    </div>
    </div>
  </div>
</main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get("https://brahmapuri-server.herokuapp.com/api/products")
      return {products:response.products}
    } catch (err) {
      console.log(err)
    }
  },
  methods: {
    async onDeleteProduct(id, index){
      try{
        let response = await this.$axios.$delete(`https://brahmapuri-server.herokuapp.com/api/products/${id}`)

        if(response.status){
          this.products.splice(index, 1)
        }
      } catch(err){
        console.log(err)
      }
    }
  }
}
</script>

<style>
body {
  background-color: rgb(255, 255, 255) !important;
}
</style>
