<template>
<div class = "webtop">
        <header>
            <h1>Welcome to Yummy Burgers Online! </h1>
        </header>
        <img clas="webtopimage" src = "https://i.pinimg.com/550x/18/de/ce/18dece998de542fba3d5bcd1faf94e58.jpg" alt="Picture of a sunset">
        </div>

        <main>
            <section id="burgerselection">
                <h2> Select your burger </h2>
                    <p> Choose between our Yummy Burgers in the menu below </p>
            
            <div class = "wrapper">

<!---loopar över burgarna i min burgararray-->   

             <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"/>

            </div>

            </section>

            <section id="orderinfo">
            <h3> Customer information</h3>
                <p> To process your order, we need some information</p>

                <h3> Delivery information </h3>

                    <form>

                        <p>
                            <label for="fullname" > Full name </label><br>
                            <input type="text" id="fullname" name="fn" required="required" placeholder="First- and Last name">
                        </p>
                        <p>
                            <label for="email">E-mail </label><br>
                            <input type="email" id="email" name="em" placeholder="E-mail address">
                        </p>
                        <p>
                            <label for="street">Street </label><br>
                            <input type="text" id="street" name="str" required="required" placeholder="Street name">
                        </p>
                            <label for="house">House </label><br>
                            <input type="number" id="house" name="house" required="required" placeholder="House number">

                    </form>

                    <p>
                        <label for="payment"> Payment </label><br>
                        <select id="payment" name="pay">
                            <option>Debit card</option>
                            <option selected = "selected">Swish</option>
                            <option>Credit card</option>
                            <option>Klarna</option>
                        </select>
                     </p>

                     <p>
                        <label for="radio"> Gender </label><br>
                            <input checked="checked" type="radio" name="gender" value="male"> Male <br>
                            <input type="radio" name="gender" value="female"> Female <br>
                            <input type="radio" name="gender" value="non-binary"> Non-binary <br>
                            <input type="radio" name="gender" value="not disclose"> Undisclosed <br>

                     </p>

                     <button type="submit">
                        <img src="https://pngimg.com/uploads/smiley/smiley_PNG36226.png" style="width: 25px">
                        Place order!
                      </button>
            </section>

        </main>
        <hr>
        <footer> 
            &COPY; Copyright 2023 Yummy Burgers AB
        </footer>

  <div>

    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const minusButton = document.getElementById("minusButton")
const plusButton = document.getElementById("plusButton")
const orderAmount = document .getElementById("orderAmount")


const socket = io();

//console.log( burgerArray)

function MenuItem(pn, url, descTitle, desc, glut, lac) {
    this.productName = pn; // The *this* keyword refers to the object itself
    this.picture = url;
    this.descriptionTitle = descTitle
    this.description = desc
    this.gluten = glut;
    this.lactose = lac;
    
}

const burgerArray = [
new MenuItem('The Green Burger', "https://cdn-bk-se-ordering.azureedge.net/media/fvidd1jj/halloumi-cheesy-cheese.png","Our Vegetarian Burger" ,"Halloumi, Avocado, Lettuce, Cheese Sauce", true, true), 
new MenuItem(' The Cheese Burger',"https://cdn-bk-se-ordering.azureedge.net/media/ywfd4uqs/cheesy-cheese.png", "Our Famous Cheese Burger","Cheese, Lettuce, Tomatoes, BBQ Sauce",true, true) , 
new MenuItem('The Chicken Burger',"https://cdn-bk-se-ordering.azureedge.net/media/tzdfpoe0/bk_kiosk_400x290_singel_crispychicken.png","Our Crispy Chicken Burger" ,"Fried Chicken, Lettuce, Tomatoes, Mild Sauce", true, false )
]


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu
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
    },



  }
}

</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: rgba(248, 196, 249, 0.66);

  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 20px;
 }

 /* add the text on top of the picture */
 .webtop img {
    width: 100%;
    height: 120px;
    border: 3px inset;
 }

 .webtop h1{
    position: absolute;
    top: 30px;
    left: 60px;
    text-align: center;
    margin: 20px ;
    z-index: 1;
    font-size: 2em;
 }


/* font for header, can't make the google fonts work*/
header {

    font-family: 'Trispace', sans-serif;
    color:#5e63b6;

}

/* class allergens*/
.allergens {

    color: #770659
}

/*Highligt the words gluten / lactose*/
#glutlac {
    font-weight: bold;
    
 }

 /* Style for the burgerselection section*/
 #burgerselection{

    /*the colorgradient below is copied from shecodes.io*/
    background: linear-gradient(to top, #9890e3 0%, #b1f4cf 100%);
    color: #27296d;
    margin: 5px;
    padding: 7px 0px 0px 10px;
    border: 10px dotted
 }

 /* Style for the delivery/orderinfo section*/
 #orderinfo {
    
    /*the colorgradient below is copied from shecodes.io*/
    background: linear-gradient(110.5deg, rgba(248, 196, 249, 0.66) 22.8%, rgba(253, 122, 4, 0.15) 64.6%);
    color: #25276a;
    margin: 5px;
    padding: 7px 0px 0px 10px;
    border: 10px double; 
 }

/*Divisions around the burgers */
div {
    padding: 10px 
 }


/*Style for the button*/

 button {
    margin: 20px;  
 }

/*Properties when hovering over the button*/
 button:hover {
    color: #f758fc; 
    cursor: pointer;    
 }

 /*Grid layout*/

 .wrapper {
    display: grid;
    grid-template-columns: 33% 33% 33%;
    margin-left: 10px; 
    margin-right: 10px;

 }

</style>