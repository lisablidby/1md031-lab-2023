<template>
  <div>
    <div>
      <h1>Burgers</h1>
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

var burgerArray = [{productName: "The Green Burger", url: "https://cdn-bk-se-ordering.azureedge.net/media/fvidd1jj/halloumi-cheesy-cheese.png", glutlac: "Contains gluten and lactose"}
                  ,{productName: "The Cheese Burger", url:"https://cdn-bk-se-ordering.azureedge.net/media/ywfd4uqs/cheesy-cheese.png", glutlac: "Contains gluten and lactose"} , 
                  {productName: "The Chicken Burger", url: "https://cdn-bk-se-ordering.azureedge.net/media/tzdfpoe0/bk_kiosk_400x290_singel_crispychicken.png", glutlac: "Containt gluten, lactosefree!"}]

console.log( burgerArray)


function MenuItem(pn, url, glutlac) {
    this.productName = pn; // The *this* keyword refers to the object itself
    this.picture = url;
    this.allegens = glutlac;
}

// det ovan har jag skrivit själv 

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850}
               ]
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}

</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;

  }
</style>