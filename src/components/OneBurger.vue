<template>
  <div class="box">
  <h3>{{ burger.name }}</h3>
  <img v-bind:src="burger.url" :alt="burger.name" style="width: 200px">
  <ul>
    <li><span>{{ burger.kCal }} kcal</span></li>
    <li v-if="burger.gluten"><span class="allergy">Contains gluten</span></li>
    <li v-if="burger.lactose"><span class="allergy">Contains lactose</span></li>
    
  </ul>
  
  <div class="button-container">
    <button class="decrement" @click="decrementOrder">-</button>
    {{amountOrdered}}
    <button class="increment" @click="incrementOrder">+</button>
    
  </div>
</div>

</template>

<script>
export default {
  name: 'OneBurger',
 
  props: {
    burger: Object
  },

  data() {
    return{
      amountOrdered: 0

    };
  },
  methods: {
    incrementOrder() {
      this.amountOrdered++;
      console.log(`Increment order: ${this.burger.name}, Amount: ${this.amountOrdered}`); // Debug log
      this.$emit('add-to-order', {name: this.burger.name, amount: this.amountOrdered})
    },
    decrementOrder() {
      if (this.amountOrdered > 0){
        this.amountOrdered--;
        console.log(`Decrement order: ${this.burger.name}, Amount: ${this.amountOrdered}`); // Debug log
        
        this.$emit('add-to-order', {name: this.burger.name, amount: this.amountOrdered})
      }
      
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); 
  gap: 20px; 
}

.box {
  background-color: #444;
  color: #fff;
  border-radius: 5px;
  padding: 20px;
  font-size: 100%;
  text-align: center;
  display: flex;
  flex-direction: column; 
  justify-content: space-between; 
  height: 100%; 
}

.allergy {
  font-weight: bold;
}

ul {
  padding: 0;
  margin: 0 auto;
}

ul li {
  margin-bottom: 10px;
}

.button-container {
  display: flex;
  justify-content: center; 
  gap: 10px; 
  margin-top: 15px; 
}

button {
  background-color: #04AA6D;
  border: none;
  color: white;
  padding: 10px 15px;
  font-size: 14px;
  border-radius: 5px;
  cursor: pointer;
}

.increment:hover {
  background-color: #08dc79;
}

.decrement {
  background-color: rgb(216, 10, 10);
}

.decrement:hover {
  background-color: rgb(255, 1, 1);
}

</style>
