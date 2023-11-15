
<template>

   

    <div class = "burgerMenu">

    <ul>
      <h2>{{ burger.productName }} </h2>
  
        <img v-bind:src="burger.picture" :alt="burger.productName">
          <ul>
              <dt> {{ burger.descriptionTitle }}</dt>
              <dd><li>{{ burger.description }} </li></dd>
                
              <section class="allergens">
                    
                <dt>Allergens</dt>
                <dd><li v-if="burger.gluten"> 
                  <span id="glutlac"> Gluten</span></li></dd>

                  <dd><li v-if="burger.lactose"> 
                  <span id="glutlac"> Lactose</span></li></dd>
                
              </section>
          </ul>
      </ul>
      
      <button id="minusButton"> - </button>
      <input type="number" value = "0" id="orderAmount">
      <button id="plusButton"> + </button>

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

  .allergens {
    color: #770659;
    }

  img {
  width:200px;
  }


  /* Each title is the same size as the text but bold */
  h2{
  font-size: 1em

  }

  
  </style>
  