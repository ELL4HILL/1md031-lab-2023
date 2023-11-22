<template>
  <div class="box">
    <h3> {{ burger.name }}</h3>
    <img :src="burger.imageURL" width="200" height="250">
    <ul>
      <li> {{burger.kcal}} calories </li>
      <li v-if="burger.lactose">contains <span class="allergiklass"> lactose </span> </li>
      <li v-if="burger.gluten" >contains <span class="allergiklass"> gluten </span> </li>
      <p> Ordered: {{amountOrdered}} </p>
          <button class="burgerButton" v-on:click="increaseOrder()">
            +
           </button>

          <button class="burgerButton" v-on:click="decreaseOrder()">
            -          
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

 .allergiklass {
  font-weight: bolder;
 }

   .wrapper {
  display: grid;
  grid-gap: 22em;
  grid-template-columns: 1px 1px 1px;
}

.burgerButton {
  margin: 1em; 
  width: 5em;
  height: 5em;
}
  </style>
  

  