<template>
  <div class="cart-list">
    <div v-for="item in spisok_dlya_rendera">
      {{ item[0] }} на {{ item[1] }} VGTB
    </div>
  </div>
</template>


<script>
import List from './List.vue';
export default {
  components: { List },
  name: 'Cart',
  props: {
    cart: Array,
    cartPrice: Number
  },
  computed: {
    spisok_dlya_rendera() { 
      let result = this.cart.reduce((a,c) => (a[c.title] = (a[c.title] || 0) + c.price*+c.amount, a), {})
      let cart = Object.entries(result)
      return cart.reverse();
    }
  },
};
</script>

<style>

</style>
