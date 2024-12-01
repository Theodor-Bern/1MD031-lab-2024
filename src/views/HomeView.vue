<template>
<header>
            
            <img src="/img/burger-rest.png" alt="Object Fit Image"> 
            <h1>Welcome to Theos Burgers</h1>   
        </header>
        
        
       
    <main>
      <section id="burger">
          <h2>Select burger</h2>
          <p>Select your burger below.</p>
          <div class="wrapper">
            <OneBurger
              v-for="(burger, index) in burgers"
              :key="index"
              :burger="burger"
            />
          </div>
      </section>
                
            <section id="contact">
    
                <h2>Customer Information</h2>
                <p>Please provide your details</p>
                <form action="/submit" method="post">
                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="firstname" name="fn" required="required" placeholder="Full name">
                </p>
                
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
                </p>
                <p>
                    <label for="street">Street</label><br>
                    <input type="text" id="street" name="st" required="required" placeholder="Street">
                </p>
                <p>
                    <label for="housenumber">Housenumber</label><br>
                    <input type="number" name="hn" id="housenumber" required="required" placeholder="Housenumber" min="1" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
                </p>
                <p>
                    <label for="Pay-method">Pay method</label><br>
                    <select id="pm" name="pay-method">
                        <option>Debit</option>
                        <option>Credit</option>
                        <option>Paypal</option>
                        <option>Venmo</option>
                        <option>Klarna</option>
                    </select>
                 </p>
                 <p>Gender:</p>
                 <p>
                     <label>
                         <input type="radio" name="gender" value="male" checked> Male
                     </label><br>
                     <label>
                         <input type="radio" name="gender" value="female"> Female
                     </label><br>
                     <label>
                         <input type="radio" name="gender" value="not_provide"> Do not wish to provide
                     </label>
                 </p>
                </form>


            </section>
        </main>

        <button type="Place your order">
            <img src="/img/buy.png" style="width:14px">
            Place your order

        </button>


        <hr>


        <footer>
            &copy; Burken's Burgershop
        </footer>
        

</template>

<script>
import OneBurger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, imageURL, kcal, gluten, lactose){
  this.name = name;
  this.imageURL =imageURL;
  this.kcal = kcal;
  this.gluten = gluten;
  this.lactose = lactose;
}

const burgers = [
  new MenuItem("Cheeseburger", "/img/cheeseburger.jpg", 1600, true, true),
  new MenuItem("Triple Onion", "/img/Onion.jpg", 1800, true, true),
  new MenuItem("The Smoky", "/img/Smoked.jpg", 2000, true, true)
];

console.log(burgers);

export default {
  name: 'HomeView',
  components: {
    OneBurger
  },
  data: function () {
    return {
      burgers
    }
  },
  mounted() {
    console.log(this.burgers); // Check if burgers array is populated
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

ul li{
    margin-bottom: 10px;
}






</style>