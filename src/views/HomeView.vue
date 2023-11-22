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
          <OneBurger v-for="burger in menuItems"
                    v-bind:burger="burger" 
                    v-bind:key="burger.name"
                    v-on:orderedBurger="addToOrder($event)"/>
          </div>
         </section>
         <section class="orderInfo">
            <h2>Order information</h2>
            <p>Insert order information</p>
            <p>
                <label for="fullname">Full name</label><br>
                <input type="text" id="fullname" v-model="fullName" required="required" placeholder="First- and Last name">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" v-model="emailAdress" required="required" placeholder="E-mail address">
            </p>
            <p>
                <label for="Payment method">Payment</label><br>
                <select id="payment" v-model="paymentMethod">
                    <option>Swish</option>
                    <option>Faktura</option>
                    <option>Kort</option>
                </select>
             </p>
             <p>
                <label for="gender">Gender</label><br>
                
                <input type="radio" id="genderChoice1" v-model="g" value="male" checked="checked" name="gender">  
                <label for="genderChoice1">Male</label> <br>
                
                <input type="radio" id="genderChoice2" v-model="g" value="female" name="gender"> 
                <label for="genderChoice2">Female</label> <br>
                
                <input type="radio" id="genderChoice3" v-model="g" value="other" name="gender"> 
                <label for="genderChoice3">Other</label><br>
                </p>
         </section>
         <div id="locationSelect">
    <h2>Choose location</h2>
      </div>
    <div class="mapdiv">
    <div id="map" v-on:click="setLocation">  
      <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }" >
        T
      </div>
    </div>
    </div>
<div>
</div>
            <button id="markDoneButton" v-on:click="markDone(fullName, emailAdress, paymentMethod, g)">
            <img src="https://strictlytoolboxes.com/wp-content/uploads/2021/11/placeorder.png" width= "200" height="200">
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
import menu from '../assets/menu.json'
import OneBurger from '../components/OneBurger.vue'
import io from 'socket.io-client'


const socket = io();

/* function MenuItem(name, imageURL, kcal, lactose, gluten) { //hur är det med booleans...?
  this.name=name;
  this.imageURL=imageURL;
  this.kcal=kcal;
  this.lactose=lactose;
  this.gluten=gluten;
}

const burger1 = new MenuItem("MummiBurger", "https://i.redd.it/g5eothuisxl11.jpg", 300, true, true);
const burger2 = new MenuItem("BubbiBurger", "https://d104wv11b7o3gc.cloudfront.net/wp-content/uploads/2016/07/low-carb-steakhouse-burger-recipe-5.jpg", 250, true, false);
const burger3 = new MenuItem("TrippleBurger", "https://www.thecookierookie.com/wp-content/uploads/2018/07/butter-burgers-recipe-8-of-8.jpg", 800, true, true);

*/



const menuItems = menu

export default {
  name: 'HomeView',
  components: {
    OneBurger
  },
  data: function () {
    return {
      menuItems,
      yourVariable: "",
      orderedBurgers:{},
      location: { x: 0,
                  y: 0
                  }
                
    }
  },
  methods: {
    markDone: function (fullName, emailAdress, paymentMethod, g) {
      this.fullName=fullName;
      this.emailAdress=emailAdress;
      this.paymentMethod=paymentMethod;
      this.g=g;
      console.log(fullName, emailAdress, paymentMethod, g);
      
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                          details: {  x: this.location.x,
                                      y: this.location.y},
                                      fullName: this.fullName,
                                      emailAdress: this.emailAdress,
                                      paymentMethod: this.paymentMethod,
                                      g: this.g,
                          orderItems: Object.entries(this.orderedBurgers)
      }
      );    
    },

    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    console.log(event.amount)
    },

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    /*addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }, 
    */

    setLocation: function (event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    }
}
}
</script>




<style>

.mapdiv {
  margin: 50px 50px 50px 225px;
  overflow: auto; 
  height: 500px; 
  width: 1000px;
}

#map {
  width: 1920px;
  height: 1078px; 
  background: url('../../public/img/polacks.jpg');
}


  #map div {
    position: relative;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }



@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@100&display=swap');


header {
    margin: 50px 50px 50px 100px;
    display: flex;
    align-items: center;
    height: 100vh;
    font-size: 60 pt;
    text-align: center;
    position: relative; 
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

#markDoneButton {
  align-self: center;
  margin-left: 50em;
  }


#locationSelect {
  margin-left: 28em;
}


 @media screen and (max-width: 800px) {
  h1 {
      font-size: 6vw;
  }
}
</style>