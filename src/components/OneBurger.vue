<template>
  <div class="box">
    <h3> {{ burger.name }} </h3>
    <img v-bind:src="burger.imageURL" :alt="burger.name" style="width: 200px">
    <ul>
      <li><span>{{ burger.kcal }} kcal</span></li>
      <li v-if="burger.gluten"><span class="allergy">Contains gluten</span></li>
      <li v-if="burger.lactose"><span class="allergy">Contains lactose</span></li>
      <li><span>Contains meat</span></li>
    </ul>

  
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
