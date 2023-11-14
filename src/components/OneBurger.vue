

<template>

    <div>
      {{ burger.name }} {{ burger.kCal }}
    </div>


  <div class = "box Green">
                
    <h4>
        The Green Burger 
    </h4>
        <img src= "https://cdn-bk-se-ordering.azureedge.net/media/fvidd1jj/halloumi-cheesy-cheese.png" alt="Halloumiburger with avocado" title="Yummy Burger's Green Burger" style="width: 210px">
            
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
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

#glutlac {
    font-weight: bold;
    
 }

  
  </style>
  