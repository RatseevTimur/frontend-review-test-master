<template>
  <div class="cart-list">
    <div v-for="item in spisok_dlya_rendera">
      {{ item }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cart: Array,
  },
  computed: {
      spisok_dlya_rendera() {//тут можно немного форматировать
          return this.cart.reverse();//Добавляется пункт списка, а не увеличиваетя количество при повторном добавлении товара
      },
  },
};
</script>

<style>

</style>
