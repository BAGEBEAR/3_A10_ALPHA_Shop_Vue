<template>
  <div class="container-cart">
    <div class="cart">
      <h1 class="cart-title d-none">購物籃</h1>
      <div class="cart-items">
        <div class="cart-item" v-for="item in cartItems" :key="item.id">
          <div class="cart-item-img">
            <img :src="item.image" alt="" />
          </div>
          <div class="cart-item-info">
            <span class="cart-item-info name">{{ item.name }}</span>
            <div class="cart-item-info-quantity">
              <div @click="removeItem(item)" class="quantity-btn minus">-</div>
              <div class="quantity-number">{{ item.quantity }}</div>
              <div @click="addItem(item)" class="quantity-btn plus">+</div>
            </div>
            <span class="cart-item-info-price">
              {{ item.price | showPriceLabel }}
            </span>
          </div>
        </div>
      </div>

      <div class="cart-count">
        <div class="cart-count-row cart-count-shipment py-4">
          <span class="shipment-title">運費</span>
          <span class="shipment-number">
            {{ deliverFee | showPriceLabel }}
          </span>
        </div>
        <div class="cart-count-row cart-count-total py-4">
          <span class="total-title">小計</span>
          <span class="total-number">
            {{ (totalNumber + deliverFee) | showPriceLabel }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { priceLabelFilter } from "../utils/mixin";

export default {
  props: {
    initialCartItems: {
      type: Array,
      required: true,
    },
    deliver: {
      type: Number,
      require: true,
    },
  },
  mixins: [priceLabelFilter],
  data() {
    return {
      cartItems: [],
      deliverFee: 0,
    };
  },
  computed: {
    totalNumber() {
      let total = 0;
      for (let i = 0; i < this.cartItems.length; i++) {
        total += this.cartItems[i].price * this.cartItems[i].quantity;
      }
      return total;
    },
  },
  watch: {
    deliver: {
      handler: function () {
        console.log("deliver change");

        this.deliverFee = Number(this.deliver) === 0 ? 0 : 500;
      },
      deep: true,
    },
  },
  methods: {
    fetchItems() {
      this.cartItems = this.initialCartItems;
    },
    addItem(item) {
      item.quantity += 1;
    },
    removeItem(item) {
      if (item.quantity > 0) {
        item.quantity -= 1;
      }
    },
    //cartItemsUpdate

    //calcTotal
  },
  created() {
    this.fetchItems();
  },
};
</script>