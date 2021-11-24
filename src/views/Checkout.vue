<template>
  <body>
    
    <div class="container">
      <div class="container-grid">
        <h1 class="container-title">結帳</h1>
        <div class="container-form">
          <CheckoutStepper 
            :step="step"
            :cartItems="cartItems"
          />
          <CheckoutForm 
            :step="step"
            @after-stepToggle="handleAfterStepToggle"
            @afterDeliverChange="handleAfterDeliverChange"
          />
        </div>
        <CheckoutList 
          :initialCartItems="cartItems" 
          :deliver="checkout.deliver"
          
        />
      </div>
    </div>
  </body>
</template>


<script>
import CheckoutStepper from "../components/CheckoutStepper.vue";
import CheckoutForm from "./../components/CheckoutForm";
import CheckoutList from "./../components/CheckoutList";

let dummyData = {
  cartItems: [
    {
      id: 1,
      name: "破壞補丁修身牛仔褲",
      quantity: 1,
      price: 3999,
      image: "https://i.imgur.com/TJDcjXQ.png",
    },
    {
      id: 2,
      name: "刷色直筒牛仔褲",
      quantity: 1,
      price: 1299,
      image: "https://i.imgur.com/CddydHx.png",
    },
  ],
};

export default {
  components: {
    CheckoutForm,
    CheckoutList,
    CheckoutStepper,
  },
  data() {
    return {
      formData: [],
      cartItems: [],
      checkout: {
        gender: "",
        name: "",
        tel: "",
        email: "",
        city: "",
        address: "",
        deliver: 0,
        payment: {
          name: "",
          cardNumber: "",
          expDate: "",
          cvc: ""
        }
      },
      step: 0
    }
  },
  methods: {
    fetchItems() {
      this.cartItems = dummyData.cartItems
    },
    handleAfterStepToggle(newStep) {
      console.log('newStep = ', newStep)
      this.step = newStep
    },
    handleAfterDeliverChange(deliver) {
      this.checkout.deliver = deliver
    }
  },
  created() {
    this.fetchItems();
  }
};
</script>