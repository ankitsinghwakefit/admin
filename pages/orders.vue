<template>
  <b-container>
    <h1>All orders placed</h1>
    <router-link to="/">Go Home Page</router-link>
    <b-row>
      <b-col xl="4" lg="4" md="6" sm="12" class="mb-2" v-for="(order,index) in orders" :key="order._id">
        <div>
          <b-card :title="order.name" :sub-title="order.address[0].phoneNumber">
            <b-card-text>
              email: {{order.email}}
            </b-card-text>

            <b-card-text>Address: {{order.address[0].address}}, <br />
              City: {{order.address[0].city}}, <br />
              State: {{order.address[0].state}}, <br />
              Pincode: {{order.address[0].zipCode}}</b-card-text>

              <b-card-text>
              <h5>Order Date: {{orderDates[index]}}</h5>
            </b-card-text>

            <b-card-text>
              Product Ordered:
            </b-card-text>

            <b-card-text v-for="(prod,index) in orderProducts[index]" :key="index">
              Name: {{prod.title}}, QTY: {{prod.qty}}
            </b-card-text>

            <a href="#" class="card-link">Payment Id: {{order.razorpayPaymentId}}</a> <br />
            <b-link href="#" class="card-link">Order Id: {{order.razorpayOrderId}}</b-link>
            <button @click="deleteOrder(order._id)" class="a-button-history margin-right-10">If order is completed, Only then Delete Order </button>
          </b-card>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data(){
    return{
      orders: null,
      orderDates: [],
      orderProducts: []
    }
  },
  mounted(){
    this.$axios.$get("https://rose-important-hedgehog.cyclic.app/api/orders").then(res=>{
      this.orders = res.orders
      this.setMyData()
      console.log("orders", this.orders)
    }).catch(err=>{
      console.log(err)
    })
  },
  methods: {
    setMyData(){
      let orderDat = []
      let orderProd = []
      this.orders.map(order=>{
        orderDat.push(order.createdAt)
        orderProd.push(order.productOrder)
      })
      console.log("orderDat", orderDat)
        orderDat.map((dat,index)=>{
        let date = new Date(dat)
        this.orderDates.push(date.toLocaleString('en-GB', {day:'numeric', month: 'long', year:'numeric'}))
      })
      this.orderProducts = orderProd
      console.log("dates", this.orderDates)
      console.log("orders after", this.orderProducts)
    },
    deleteOrder(orderId){
      this.$axios.$delete(`https://rose-important-hedgehog.cyclic.app/api/orders/${orderId}`).then(res=>{
        console.log("res", res)
        if(res.success){
          alert("Order has been deleted...")
          // this.$forceUpdate();
        }
      }).catch(err=>{
        console.log(err)
      })
    }
  }
}
</script>