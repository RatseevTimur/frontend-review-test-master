<script>
export default {
  name: 'Header',
  props: {
    cart: Array,
    required: true,
    default: {},

    currency: String,
  },
  data() {
    return {
      cartPrice: 0,
    };
  },
  watch: {
    cart(cart) {
      let val = 0;
      cart.forEach((item) => {
        val += item.price * item.amount; //Товаров в корзине на: 104.99000000000001 VGTB - если заказать картошку и моркову по 1кг (нужно устранить потерю точности)
      });
      this.cartPrice = val;
    },
  },
}
</script>

<style scoped>
  .header {
    padding: 10px;
    background: #e5e5e5;
  }
</style>

<template>
  <div class="header">
    <h3>Товаров в корзине на: {{ cartPrice }} {{ currency }}</h3>
  </div>
</template>
