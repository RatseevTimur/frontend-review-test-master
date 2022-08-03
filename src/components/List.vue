<template>
  <div class="product-list">
    <div class="card" v-for="product in products" :style="{'width': (100/cardsWidth) - 3 + '%'}">
      <p class="card-title">{{ product.title }}</p>
      <img class="card-image" :src="product.image" alt="">
      <p class="card-price">Цена: {{ product.price }} {{ currency }}</p>

      <div>
        <input type="number" min="0" ref="amount" :id="product.id">
        <span>кг</span>
      </div>

      <button @click="addToCart(product)"> В корзину </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currency: String,
  },
  data() {
    return {
      products: [],
      width: window.innerWidth
    };
  },
  computed: {
    cardsWidth(){
      if (this.width >= 840) {
        return 3;
      } 
      if (this.width >= 420 && this.width < 840) {
        return 2;
      }
      return 1
    }
  },
  methods: {
    async getList() {
      let data = await this.$store.dispatch('getProductsList');
      this.products = data;
    },
    addToCart(product) {
      let amount = this.$refs.amount.find((input) => input.id === product.id)
      if (amount.value > 0){
        let data = {
          amount: amount.value,
          price: product.price,
          title: product.title,
        };
        this.$parent.cart.push(data);
        amount.value = ""
      }
    }
  },
  created() {
    this.getList();
  },
};
</script>

<style>
  .product-list {
    display: flex;
    padding: 10px;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .card {
    display: inline-block;
    border: 1px solid #908888;
    border-radius: 5px;
    margin: 1%;
    text-align: center;
    padding: 10px;
  }
  .card-image {
    width: 100%;
  }
  button {
    padding: 5px;
    margin: 5px;
  }

</style>
