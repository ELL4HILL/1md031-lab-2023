<template>
  <div>
    <header>
        <h1>WELCOME TO MAMAS BURGERS</h1>
        <img src="https://img.freepik.com/premium-vector/fast-food-background-linear-graphic-snack-collection-junk-food-engraved-top-view-illustration-vector-illustration_91128-1528.jpg" class="header-img">
     </header>
    <main>
    <section>
        <h2>Select burger</h2>
        <p>Choose what type of burger you want to order</p>
          <div class="wrapper">
          <OneBurger v-for="burger in menuItems" :key="burger.name" :burger="burger" />
          </div>
         </section>
         <section class="orderInfo">
            <h2>Order information</h2>
            <p>Insert order information</p>
            <p>
                <label for="fullname">Full name</label><br>
                <input type="text" id="fullname" name="fn" required="required" placeholder="First- and Last name">
            </p>
            <p>
                <label for="street">Street </label><br>
                <input type="text" id="street" name="s" placeholder="Street name">
            </p>
            <p>
                <label for="housenumber">House</label><br>
                <input type="number" id="housenumber" name="hn" placeholder="House number">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
            </p>
            <p>
                <label for="Payment method">Payment</label><br>
                <select id="payment" name="pm">
                    <option>Swish</option>
                    <option>Faktura</option>
                    <option>Kort</option>
                </select>
             </p>
             <p>
                <label for="gender">Gender</label><br>
                <input type="radio" id="genderChoice1" name="g" value="male" checked="checked">
                <label for="genderChoice1">Male</label> <br>
                <input type="radio" id="genderChoice2" name="g" value="female"> 
                <label for="genderChoice1">Female</label> <br>
                <input type="radio" id="genderChoice3" name="g" value="other"> 
                <label for="genderChoice1">do not whish to provide</label><br>
                </p>
         </section>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  <input type="text" v-bind:value="yourVariable" v-on:input="yourVariable = $event.target.value">
<div>
  {{ yourVariable }}
  {{menu}}
</div>
         <button type="submit">
            <img src="https://strictlytoolboxes.com/wp-content/uploads/2021/11/placeorder.png" width= "100" height="100">
            PLACE ORDER
          </button>
    </main>
    <hr>
    <footer>
    &copy;2010bipomama
    </footer>
  </div>
</template>

<script>
import OneBurger from '../components/OneBurger.vue'
import menu from '.../assets/menu.json'
import io from 'socket.io-client'


const socket = io();

function MenuItem(name, imageURL, kcal, lactose, gluten) { //hur är det med booleans...?
  this.name=name;
  this.imageURL=imageURL;
  this.kcal=kcal;
  this.lactose=lactose;
  this.gluten=gluten;
}

const burger1 = new MenuItem("MummiBurger", "https://i.redd.it/g5eothuisxl11.jpg", 300, true, true);
const burger2 = new MenuItem("BubbiBurger", "https://d104wv11b7o3gc.cloudfront.net/wp-content/uploads/2016/07/low-carb-steakhouse-burger-recipe-5.jpg", 250, true, false);
const burger3 = new MenuItem("TrippleBurger", "https://www.thecookierookie.com/wp-content/uploads/2018/07/butter-burgers-recipe-8-of-8.jpg", 800, true, true);

const menuItems = []
menuItems.push(burger1,burger2, burger3);

export default {
  name: 'HomeView',
  components: {
    OneBurger
  },
  data: function () {
    return {
      menuItems,
      yourVariable: ""
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

@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@100&display=swap');


  

header {
    margin: 50px 50px 50px 100px;
    display: flex;
    align-items: center;
    height: 100vh;
    font-size: 60 pt;
    text-align: center;
    position: relative; /* Lägg till position: relative för att .header-img ska positionera sig inuti detta element */

  }

  .header-img {
    position: absolute;
    width: 100%; 
    height: 100%;
    opacity: 0.25;
    z-index: -1;
  }
  

  h1 {
    font-size: 5em; 
    color: w;
  }
  

section {
    margin: 50px 50px 50px 100px;
    border: 2px dotted #ff9900;
    padding-left: 2em;
    padding-bottom: 5em;
  }

  .wrapper {
  display: grid;
  grid-gap: 22em;
  grid-template-columns: 1px 1px 1px;
}
  

 .orderInfo {
    color: white;
    background-color: black;
 }

  h3:hover {
    color: palevioletred;
  }

  button{
    margin: 50px 50px 50px 100px;
  }
  button:hover {
    background-color:pink ;
    cursor:pointer;
 }


 @media screen and (max-width: 800px) {
  h1 {
      font-size: 6vw;
  }
}
</style>