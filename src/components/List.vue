<template>
  <div class="product-list">
    <div class="card" v-for="product in products" :style="{width: cardsWidth + '%'}">
      <!--Я бы реализовал подобный адаптив на CSS, но в данном случае подправим этот код-->
      <p class="card-title">{{ product.title }}</p>
      <img class="card-image" :src="product.image" alt="">
      <p class="card-price">Цена: {{ product.price }} {{ currency }}</p>

      <div>
        <input type="number" ref="amount" :id="product.id">
        <span>кг</span>
      </div>

      <button @click="addToCart(product)"> В корзину </button> <!--Кнопка срабатывает всегда, даже если количестов =0 или отрицательное-->
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
      /*Я бы создал массив товаров, и вносил в него изменения при заказе
      Разместил бы содержимое компонентов иначе, но на практике следует исправлять, а не писать проект заново*/
    };
  },
  computed: {
    cardsWidth() {// Данный способ не обеспечит реактивности, но этого достаточно что бы определить размер экрана при первой загрузке
      let width = window.innerWidth;//Лучше вынести данне значение в data
      let count = 1;
      if (width > '840px') {//width значение являетя числовым и без рх
        count = 3;
      } else if ((width > '420px' && width < '840px')) {
        count = 2;
      }

      return 100 / count;
    },
  },
  methods: {
    startPricesMonitoring() {//в данном случае нет смысла в этой операции
      setInterval(this.getList, 1000);
    },
    async getList() {
      let data = await this.$store.dispatch('getProductsList');

      this.products = data;
    },
    addToCart(product) {
      let amount = this.$refs.amount.find((input) => input.id === product.id).value;

      let data = {
        amount,
        price: product.price,
        title: product.title,
      };
      this.$parent.cart.push(data);
      //Тут можно добавить очистку инпута после добавления товара
    },
  },
  created() {
    this.startPricesMonitoring();
  },
};
</script>

<style>
/*Добавим Flex для выполнения ТЗ*/
  .product-list {
    padding: 10px;
  }

  .card {
    display: inline-block;
    width: 100%;/*Тут дынный параметр не работает и не нужен*/
    border: 1px solid #908888;
    border-radius: 5px;
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
