<template>
  <div class="box">
    <h3> {{ burger.name }}</h3>
    <img :src="burger.imageURL" width="200" height="250">
    <ul>
      <li> {{burger.kcal}} kalorier </li>
      <li v-if="burger.lactose">Innehåller laktos</li>
      <li v-if="burger.gluten" >Innehåller <span class="glutenklass"> gluten </span> </li>
      antal beställda burgare: {{amountOrdered}}
          <button class="burgerButton" v-on:click="increaseOrder()">
            Add burger
          </button>

          <button class="burgerButton" v-on:click="decreaseOrder()">
            Remove burger
          </button>
    </ul>
    </div>
</template>

  //  
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
  return {
    amountOrdered: 0,
  } 
  },
  methods: {
    increaseOrder() {
      this.amountOrdered++
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                    amount: this.amountOrdered} );
    }, 

    decreaseOrder() {
      if (this.amountOrdered>0)
      (this.amountOrdered--)
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                    amount: this.amountOrdered} );
    }

  }
  
}

  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

.box {
  background-color: #f0e3e9;
  color: rgb(0, 0, 0);
  border-radius: 50px;
  padding: 20px;
  width: 12em;
}

 .glutenklass {
  font-weight: bolder;
 }

   .wrapper {
  display: grid;
  grid-gap: 22em;
  grid-template-columns: 1px 1px 1px;
}

.burgerButton {
  margin: 0em 0em 0em 0em; 
}
  </style>
  

  