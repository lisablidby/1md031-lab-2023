
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
    
    <div class = "orderButtons">
      <button v-on:click="burgerRemoved"> - </button>
      <span> {{ amountOrdered }} </span>
      <button v-on:click="burgerAdded"> + </button>
    </div>
    </div>
  </template>
  
  <script>


export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    
  data: function () {
      return {
        amountOrdered: 0,
  }},


methods: {

  burgerAdded: function(){
     this.amountOrdered ++;
     this.$emit('orderedBurger', { name: this.burger.productName, 
                                  amount: this.amountOrdered 
                              }
                );
    },

  burgerRemoved: function(){
    if(this.amountOrdered>0){
    this.amountOrdered -- ;
    this.$emit('orderedBurger', { name: this.burger.productName, 
                                  amount: this.amountOrdered 
                              }
                );
    }
  },
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


  /* Putting the buttons in the center of each box */
  .orderButtons{
    margin-left: 60px;

  }

  
  </style>
  