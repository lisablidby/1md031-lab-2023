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
                <div class = "box Green">
                
                <h4>
                    The Green Burger 
                </h4>
                <img src= "https://cdn-bk-se-ordering.azureedge.net/media/fvidd1jj/halloumi-cheesy-cheese.png" alt="Halloumiburger with avocado" title="Yummy Burger's Green Burger" style="width: 210px" >
            
            <ul>
                <dt> Our Vegetarian Burger </dt>
                <dd><li>Halloumi </li></dd>
                <dd><li>Avocado</li></dd>
                <dd><li>Cheese sauce</li></dd>
                

                <section class="allergens">
                    
                    <dt>Allergens</dt>
                    <dd><li>Contains <span id="glutlac">gluten</span></li></dd>
                    <dd><li>Contains <span id="glutlac">lactose</span> </li></dd>
                    
                </section>
            </ul>
            </div>
            <div class = "box cheese">
                
                <h4>
                    The Cheese Burger 
                </h4>
                <img src= "https://cdn-bk-se-ordering.azureedge.net/media/ywfd4uqs/cheesy-cheese.png" alt="Picture of Cheese Burger" title="Yummy Burger's Cheese Burger" style="width: 200px">

            <ul>
                <dt> Our Famous Cheese Burger </dt>
                <dd><li>Spicy or mild cheese </li></dd>
                <dd><li>Lettuce and tomatoes</li></dd>
                <dd><li>BBQ sauce</li></dd>
                
                <section class="allergens">
                    
                    <dt>Allergens</dt>
                    <dd><li>Contains <span id="glutlac">gluten</span></li></dd>
                    <dd><li>Contains <span id="glutlac">lactose</span></li></dd>
                    
                </section>
            </ul>
            </div>

            <div class = "box chicken">
                
                <h4>
                    The Chicken Burger 
                </h4>
                <img src= "https://cdn-bk-se-ordering.azureedge.net/media/tzdfpoe0/bk_kiosk_400x290_singel_crispychicken.png" alt="Picture of Chicken Burger" title="Yummy Burger's Chicken Burger" style="width: 250px" >

            <ul>
                <dt> Our Fresh Chicken Burger </dt>
                <dd><li>Fried chicken </li></dd>
                <dd><li>Lettuce and tomatoes</li></dd>
                <dd><li> Mild sauce</li></dd>
                
                <section class="allergens">
                    
                    <dt>Allergens</dt>
                    <dd><li>Contains <span id="glutlac">gluten</span></li></dd>
                    <dd><li><span id="glutlac">Lactose free! </span></li></dd>
                </section>
            </ul>
            </div>
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

// det ovan har jag skrivit själv och det skrivs ut om man kollar console när man inspekterar ! :)

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