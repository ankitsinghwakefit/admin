<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align: center">Update product by entering all the below fields</h2>
            <form>
              <!-- Category dropdown -->
              <!-- <div class="a-spacing-top-medium">
                <label>Category</label>
                <select v-model="categoryID" class="a-select-option">
                  <option v-for="category in categories" :value="category._id" :key="category._id">{{category.type}}</option>
                </select>
              </div> -->
              <!-- Owner dropdown -->
              <!-- <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select v-model="ownerID" class="a-select-option">
                  <option v-for="owner in owners" :value="owner._id" :key="owner._id">{{owner.name}}</option>
                </select>
              </div> -->
              <!-- title -->
              <div class="a-spacing-top-medium">
                <label>Title</label>
                <input v-model="title" style="width: 100%" type="text" class="a-input-text" :placeholder="product.title">
              </div>
              <!-- Price -->
              <div class="a-spacing-top-medium">
                <label>Price</label>
                <input v-model="price" style="width: 100%" type="text" class="a-input-text" :placeholder="product.price">
              </div>
              <!-- stockQuantity -->
              <div class="a-spacing-top-medium">
                <label>Stock Quantity</label>
                <input v-model="stockQuantity" style="width: 100%" type="text" class="a-input-text" :placeholder="product.stockQuantity">
              </div>
              <!-- Description -->
              <div class="a-spacing-top-medium">
                <label>Description</label>
                <textarea v-model="description" :placeholder="product.description" style="width: 100%" />
              </div>
                 <!-- Photo file -->
                <div class="a-spacing-top-medium">
                <label>Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected"/>
                    <p style="width: max-content">{{fileName}}</p>
                  </label>
                </div>
              </div>
              <!-- button -->
              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct">Update Product</span>
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    try {
    // let categories = $axios.$get("http://localhost:8000/api/categories")
    // let owners = $axios.$get("http://localhost:8000/api/owners")
    let product = $axios.$get(`https://brahmapuri-server.herokuapp.com/api/products/${params.id}`)
    const [productReseponse] = await Promise.all([
      product
    ])
    return {
      product: productReseponse.product
    }
    } catch (err) {
      console.log(err)
    }
  },

  data(){
    return{
      title: "",
      price: "",
      description: "",
      stockQuantity: "",
      selectedFile: null,
      fileName: ""
    }
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0]
      this.fileName = event.target.files[0].name
      console.log("selected file",this.selectedFile)
      console.log("fileName", this.fileName)
    },
    async onUpdateProduct() {
     try {
        let data = new FormData()
      data.append("title", this.title)
      data.append("price", this.price)
      data.append("description", this.description)
      data.append("stockQuantity", this.stockQuantity)
       data.append("photo", this.selectedFile, this.selectedFile.name)
     
      // let formData = {}
      // formData.title = this.title
      // formData.price = this.price
      // formData.description = this.description
      // formData.ownerID = this.ownerID
      // formData.stockQuantity = this.stockQuantity
      // formData.categoryID = this.categoryID
      // formData.photo = this.fileName
      // data.append("photo", this.selectedFile, this.selectedFile.name)
      let result = await this.$axios.$put(`https://brahmapuri-server.herokuapp.com/api/products/${this.$route.params.id}`, data)
      // console.log("result", result)
      this.$router.push("/")
    } catch (err) {
      console.log(err)
    }
     }
  }
}
</script>

