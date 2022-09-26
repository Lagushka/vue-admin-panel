<template>
  <div id="app" @click="removeMenus">
    <PageHeader />
    <main>
      <div class="orders">
        <h2>Заказы</h2>
        <button class="refresher">        
          <img src="./assets/refresh.svg" alt="" width="35" @click="getOrders()">
        </button>
        <ul class="list">
          <li class="order" v-for="order in orders" :key="order.id">
            <Order 
              :element="order" 
              :delivery="false" 
              @changeVisibility="changeVisibility" 
              @showInfo="showMore"
              @toDelivery="orderToDelivery" 
              @removeElement="deleteElement"
            />
          </li>
        </ul>
      </div>
      <div class="deliveries">
        <h2>Отгрузки</h2>
        <button class="refresher">
          <img src="./assets/refresh.svg" alt="" width="35" @click="getDeliveries()">
        </button>
        <ul class="list">
          <li class="delivery" v-for="delivery in deliveries" :key="delivery.id">
            <Order 
              :element="delivery" 
              :delivery="true"
              @changeVisibility="changeVisibility"
              @removeElement="deleteElement"
            />
          </li>
        </ul>
      </div>
    </main>
    <PageFooter />
    <ModalWindow :element="element" v-if="openModal" @click="openModal = false" @closeModal="openModal = false" @toDelivery="orderToDelivery" @delete="deleteElement" />
  </div>
</template>

<script>
import PageHeader from "./components/PageHeader.vue";
import Order from "./components/OrderBrief.vue";
import axios from 'axios';
import ModalWindow from './components/ModalWindow.vue';
import PageFooter from "./components/PageFooter.vue";

export default {
  data() {
    return {
      orders: [],
      deliveries: [],
      openModal: false,
      element: {},
    };
  },
  components: {
    PageHeader,
    Order,
    ModalWindow,
    PageFooter,
},
  methods: {
    async getOrders() {
      axios
        .get('http://localhost:3000/orders')
        .then(response => (this.orders = response.data))
        .then(() => {
          for (const order of this.orders) {
            order['menuVisibility'] = false;
          }
        });
    },

    async getDeliveries() {
      axios
        .get('http://localhost:3000/deliveries')
        .then(response => (this.deliveries = response.data))
        .then(() => {
          for (const delivery of this.deliveries) {
            delivery['menuVisibility'] = false;
          }
        });
    },

    removeMenus(element) {
      if (element) {
        if (element.order) {
          console.log('hello');
          for (const order of this.orders) {
            order['menuVisibility'] = false;
          }
          for (const delivery of this.deliveries) {
            if (delivery.id != element.id) {
              delivery['menuVisibility'] = false;
            }
          }
        } else {
          for (const order of this.orders) {
            if (order.id != element.id) {
              order.menuVisibility = false;
            }
          }
          for (const delivery of this.deliveries) {
            delivery.menuVisibility = false;
          }
        }
      } else {
        for (const order of this.orders) {
          order['menuVisibility'] = false;
        }
        for (const delivery of this.deliveries) {
          delivery['menuVisibility'] = false;
        }
      }
    },

    changeVisibility(element) {
      this.removeMenus(element);
      element.menuVisibility = !element.menuVisibility;
    },

    showMore(element) {
      this.openModal = true;
      this.element = element;
    },

    async orderToDelivery(element) {
      axios
      .get('http://localhost:3000/deliveries/'+element.id)
      .then(
          () => {
              console.log('Эта отгрузка уже существует') 
          },
          async () => {
              let date = new Date();
              let day = date.getDate().toString();
              if (day.length < 2) {
                  day = '0' + day;
              }
              let month = (date.getMonth() + 1).toString();
              if (month.length < 2) {
                  month = '0' + month;
              }
              let year = date.getFullYear().toString();
              const requestBody = JSON.stringify({
                  id: element.id,
                  cost: element.cost,
                  order: element.id,
                  date: day+'.'+month+'.'+year,
                  menuVisibility: false,
                  products: element.products,
              });
              const config = {
                  headers: {
                      'Content-Type': 'application/json',
                  }
              };
              const result = await axios.post('http://localhost:3000/deliveries', requestBody, config);
              console.log(result);
              this.getOrders();
              this.getDeliveries();
          },
      );
    },

    async deleteElement(element) {
      if (element.order) {
          const result = await axios.delete('http://localhost:3000/deliveries/'+element.id);
          console.log(result);
      } else {
          const result = await axios.delete('http://localhost:3000/orders/'+element.id);
          console.log(result);
      }
      this.getOrders();
      this.getDeliveries();
    },
  },
  mounted() {
    this.getOrders();
    this.getDeliveries();
    document.documentElement.setAttribute('theme', 'light');
    window.addEventListener("keydown", (event) => {
      if (this.openModal && event.key == "Escape") {
        this.openModal = false; 
      }
    });
  }
};
</script>
<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');

  html[theme='light'] {
    $mainColor: white;
    $secondColor: black;

    body {
    background-color: $mainColor;
    color: $secondColor;
    border-color: $secondColor;

      button {
        background-color: $mainColor;
        color: $secondColor;
        border-color: $secondColor;
        cursor: pointer;
      }
    }
  }

  html[theme='dark'] {
    $mainColor: black;
    $secondColor: white;

    body {
      background-color: $mainColor;
      border-color: $secondColor;
      color: $secondColor;

      button {
        background-color: $mainColor;
        border-color: $secondColor;
        color: $secondColor;
      }
    }
  }

  html {
    margin: 0;

    body {
      display: flex;
      justify-content: center;
    }

    button {
      cursor: pointer;
    }
  }

  #app {
    font-family: 'Inter', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    max-width: 1300px;
  }

  main {
    z-index: 0;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    box-sizing: border-box;
    height: 580px;
    margin-top: 10px;

    @media (max-width: 1300px) {
      height: fit-content;
      flex-direction: column;

      .deliveries {
        margin-top: 10px;
      }
    }

    .orders, .deliveries {
      position: relative;
      box-sizing: border-box;
      width: 49%;
      border: 3px solid;

      @media (max-width: 1300px) {
        width: 100%;
      }

      h2 {
        text-align: center;
      }

      button {
        height: 40px;
        width: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        border: 3px solid; 
      }

      .refresher {
        top: 10px;
        position: absolute;
        background-color: white;
      }
    }

    .orders button {
      right: 10px;
    }

    .deliveries button {
      left: 10px;

      @media (max-width: 1300px) {
        left: auto;
        right: 10px;
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
