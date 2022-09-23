<template>
  <div id="app" :class="{ 'dark-theme': darkTheme }">
    <PageHeader @changeTheme="(theme) => (darkTheme = theme)" />
    <main>
      <div class="orders">
        <h2>Заказы</h2>
        <ul class="list">
          <li class="order" v-for="order in orders" :key="order.id">
            <Order :id ="'Заказ #'+order.id" :cost = "order.cost.toLocaleString('en')" />
          </li>
        </ul>
      </div>
      <div class="deliveries">
        <h2>Отгрузки</h2>
        <ul class="list">
          <li class="delivery" v-for="delivery in deliveries" :key="delivery.id">
            <Order :id ="'Отгрузка #'+delivery.id" :cost = "delivery.cost.toLocaleString('en')" />
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import PageHeader from "./components/PageHeader.vue";
import Order from "./components/OrderBrief.vue";
import axios from 'axios';

export default {
  data() {
    return {
      darkTheme: false,
      orders: [],
      deliveries: [],
    };
  },
  components: {
    PageHeader,
    Order,
  },
  methods: {
    async getData() {
      const ordersList = await axios.get('http://localhost:3000/orders');
      this.orders = ordersList.data;
      const deliveriesList = await axios.get('http://localhost:3000/deliveries');
      this.deliveries = deliveriesList.data;
    }
  },
  mounted() {
    this.getData();
  }
};
</script>
<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');

  #app {
    font-family: 'Inter', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  main {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 1300px;

    .orders, .deliveries {
      width: 49%;
      border: 2px solid black;

      h2 {
        text-align: center;
      }
    }

    .list {
      list-style-type: none;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0;

      .order, .delivery {
        margin: 1em 0 0 0;
      }
    }
  }
</style>
