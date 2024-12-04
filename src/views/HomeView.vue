<template>
  <header>
              
              <img src="/img/burger-rest.png" alt="Object Fit Image"> 
              <h1>Welcome to Theos Burgers</h1>   
          </header>
          
          
         
      <main>
        <section id="burger">
            <h2>Menu</h2>
            <p>Select your burger below.</p>
            <div class="wrapper">
              <OneBurger
                v-for="burger in burgers"
                v-bind:key="burger.name"
                v-bind:burger="burger"
                v-on:add-to-order="addToOrder($event)"
              />
            </div>
        </section>
                  
          <section id="contact">
      
              <h2>Customer Information</h2>
              <p>Please provide your details</p>
              <form action="/submit" method="post">
              <p>
                  <label for="fullname">Full name</label><br>
                  <input type="text" id="fullname" v-model="formData.fullname" placeholder="Full name">
              </p>
                  
              <p>
                  <label for="email">E-mail</label><br>
                  <input type="email" id="email" v-model="formData.email" placeholder="E-mail address">
              </p>
              
              <p>
                  <label for="Pay-method">Pay method</label><br>
                  <select id="payMethod" v-model="formData.payMethod">
                      <option>Debit</option>
                      <option>Credit</option>
                      <option>Paypal</option>
                      <option>Venmo</option>
                      <option>Klarna</option>
                  </select>
              </p>
              <p>
                  Gender:
                  <label>
                      <input type="radio" v-model="formData.gender" value="male" /> Male
                  </label>
                  <label>
                      <input type="radio" v-model="formData.gender" value="female" /> Female
                  </label>
                  <label>
                      <input type="radio" v-model="formData.gender" value="not_provide" /> Do not wish to provide
                  </label>
              </p>
                  
              </form>
              <p class="map-text"> Please point out your wished adress for your delivery on the map below</p>
              <div id="map-container">
                  <div id="map" @click="setLocation">
                      <div v-if="location.x !== 0 || location.y !== 0" class="dot"
                              :style="{ left: location.x + 'px', top: location.y + 'px' }">
                              T
                      </div>
                  </div>
              </div>
  
          </section>
          
  
          <form @submit.prevent>
              <button type="button" @click="addOrder">
                  <img src="/img/buy.png" style="width:14px">
                      Place your order
              </button>
          </form>
          </main>
          <hr>
          <footer>
              &copy; Theo's Burgershop
          </footer>
          
  </template>
  
  <script>
  import OneBurger from '../components/OneBurger.vue'
  import io from 'socket.io-client'
  import menu from '../assets/menu.json';
  
  const socket = io("localhost:3000");
  
  function MenuItem(name, imageURL, kcal, gluten, lactose){
    this.name = name;
    this.imageURL =imageURL;
    this.kcal = kcal;
    this.gluten = gluten;
    this.lactose = lactose;
  }
  
  const burgers = menu;
  
  console.log(burgers);
  
  
  export default {
    name: 'HomeView',
    components: {
      OneBurger,
    },
    data: function () {
      return {
        burgers,
        orderedBurgers: {},
        location: { x: 0, y: 0 },
        formData: {
          fullname: '',
          email: '',
          payMethod: 'Debit',
          gender: 'male',
        },
      };
    },
    
    methods: {
      addToOrder(event) {
        this.orderedBurgers[event.name] = event.amount;
        console.log(this.orderedBurgers);
      },
      getOrderNumber: function () {
        return Math.floor(Math.random() * 100000);
      },
      setLocation(event) {
        const rect = event.currentTarget.getBoundingClientRect();
  
        this.location.x = event.clientX - rect.left;
        this.location.y = event.clientY - rect.top;
      },
      addOrder: function () {
      let order = {
        fullname: this.formData.fullname,
        email: this.formData.email,
        paymentMethod: this.formData.payMethod,
        gender: this.formData.gender,
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers
      };
      socket.emit("addOrder", order);
    },
  }

  }
  
      
      
     
    
  
  </script>
  
  <style>
   @import url(https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&family=Play&family=Roboto:wght@400;500;700&display=swap);
  body {
      font-size: 14pt;
      font-family: 'Josefin Sans', sans-serif;
  }
  
  section{
      margin: 20px 4px;
      padding: 10px 23px 10px;
      border-radius: 8px;
      
  
  }
  #burger{
      border: dashed;
  }
  
  #contact{
      color: white;
      background-color: black;
      border: dashed;
      border-color: white;
  
  }
  
  #burger div{
      margin: 5px 16px;
      padding: 50px;
      align-items: center;
  }
  
  
  
  
  button{
      background-color: #04AA6D;
      border: none;
      color: white;
      padding: 15px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 30px 4px;
      border-radius: 6px;
      cursor: pointer;
  }
  
  button:hover{
      background-color: green;
  }
  
  nav{
      margin: 10px 4px;
  }
  
  header{
      width: 100%;
      height: 300px;
      overflow: hidden;
      margin: 0 4px;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;   
  }
  
  h1{
      position: absolute;
      padding: 100pt;   
  }
  
  header img{
      width: 100%;
      height: auto;
      object-fit: cover;
      opacity: 0.7;
  }
  
  .wrapper {
      display: grid;
      grid-template-columns: 33% 33% 33%;
      background-color: #fff ;
      color : #444;
  }
  
  
  .box{
      background-color: #444;
      color: #fff;
      border-radius: 5px;
      padding: 20px;
      font-size: 100%;
  }
  
  .allergy{
      font-weight: bold;
  }
  
  .map-text{
      font-size: 30px;
      color: red;
  }
  
  
  #map {
    position: relative;
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg") no-repeat center center;
    background-size: cover;
    cursor: crosshair;
      
  }
  #map-container{
        
    overflow: scroll;
    width: 100%;
    height: auto;
  
  }
  
  .dot {
    position: absolute;
    width: 25px;
    height: 25px;
    background-color: red;
    color: white;
    text-align: center;
    line-height: 20px;
    border-radius: 50%;
    font-size: 16px;
    font-weight: bold;
  }
  ul li{
      margin-bottom: 10px;
  }
  
  
  
  
  
  
  </style>