<template>
  <div class="box">
    <h3> {{ burger.name }}</h3>
    <img :src="burger.imageURL" width="200" height="250">
    <ul>
      <li> {{burger.kcal}} kalorier </li>
      <li v-if="burger.lactose">Innehåller laktos</li>
      <li v-if="burger.gluten" >Innehåller <span class="glutenklass"> gluten </span> </li>
      <!-- Andra detaljer om burgaren beroende på dess egenskaper -->
    </ul>
  </div>
</template>

  //  
  
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
  </style>
  

  