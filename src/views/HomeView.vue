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
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>

            </div>

            </section>

            <section id="orderinfo">
            <h3> Customer information</h3>
                <p> To process your order, we need some information</p>

                <h3> Delivery information </h3>

                    <form>

                        <p>
                            <label for="fullname" > Full name </label><br>
                            <input type="text" id="fullname" v-model="nameText" required="required" placeholder="First- and Last name">
                        </p>
                        <p>
                            <label for="email">E-mail </label><br>
                            <input type="email" id="email" v-model="emailText" placeholder="E-mail address">
                        </p>

                    </form>

                    <p>
                        <label for="payment"> Payment </label><br>
                        <select id="payment" v-model="paymentPicked">
                            <option>Debit card</option>
                            <option >Swish</option>
                            <option>Credit card</option>
                            <option>Klarna</option>
                        </select>
                     </p>

                     <p>
                        <label for="radio"> Gender </label><br>
                            <input type="radio" value="male" v-model="genderPicked" > Male <br>
                            <input type="radio" value="female" v-model="genderPicked" > Female <br>
                            <input type="radio"  value="non-binary" v-model="genderPicked"> Non-binary <br>
                            <input type="radio" value="undisclosed" v-model="genderPicked"> Undisclosed <br>
                            
                     </p>

                <h4>Please click on your location on the map </h4>

                  <section id="mapShown">
                    <div id="map" v-on:click="setLocation">

                      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
                        <div v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}" v-bind:key="'dots' + key">
                        T
                        </div>
                      </div>
                          
                    </div>
                  </section>

                     <button type="submit" v-on:click="submitOrder">
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


  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();

function MenuItem(pn, url, descTitle, desc, glut, lac) {
    this.productName = pn; 
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
      burgers: menu,
      nameText: null,
      emailText: null,
      paymentPicked:"Swish",
      genderPicked: "female",
      orderedBurgers: {},
      location: { x: 0, y: 0}
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    // this happens when you click on "submit your order"
    submitOrder:function (){
     socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x ,
                                           y: this.location.y },

                                orderItems: this.orderedBurgers,
                              

                                customerInfo: {customerName: this.nameText, 
                                              customerGender: this.genderPicked,
                                              customerEmail: this.emailText,
                                              customerPay: this.paymentPicked

                                }
                                            
                            
                              },
                 );
    },

    //this happens when you press + or - 
    addToOrder: function (event) {
    console.log(this.orderedBurgers[event.name] = event.amount,
                this.orderedBurgers
      );
    },

    // saves the location the customer clicks on the map 
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      this.location = { x: event.clientX - 10 - offset.x,
                        y: event.clientY - 10 - offset.y }

    },

},
}

</script>

<style>


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

 #map {
    width: 1920px;
    height: 1070px;
    background: url(../../public/img/polacks.jpg);
   /*background: url(/Users/lisa/Documents/Hamburgarprojektet/1md031-lab-2023/public/img/polacks.jpg);*/

  }
  
  #mapShown{
    display:grid;
    overflow: scroll;
    width: 100%;
    height: 350px;

  }

  #map {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }





</style>