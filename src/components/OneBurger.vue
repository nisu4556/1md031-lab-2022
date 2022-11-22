<template>
  <div>
    <section class="wrapper">
      <div v-bind:class="burger.nr" class="burg" >
        <h3 class="rub2">{{burger.name}}</h3>
        <img id="pic_burgar" v-bind:src="burger.img">
          <ul>
            <li>Chesse</li>
            <li>150g Beef</li>
            <li>{{burger.kCal}} kCal</li>
            <li class="allergy">Has lactose {{burger.lactose}}</li>
            <li class="allergy">Has gluten {{burger.gluten}}</li>
            <li>lettuce</li>
          </ul>
        <section><p>Chosen burgers: {{ amountOrdered }}</p></section>
        <button type="button" v-on:click="addBurgers"> Add</button>
        <button type="button" v-on:click="removeBurgers">remove</button>
        </div>
      </section>
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
      }
    },
    methods:{
      addBurgers: function(){
        this.amountOrdered += 1;
        console.log(this.burger)
        console.log(this.amountOrdered)
        console.log(this.burger.name)


        this.$emit('orderedBurgers', {name: this.burger.name, amount: this.amountOrdered});
      },
      removeBurgers: function(){
        if(this.amountOrdered > 0){
          this.amountOrdered -=1;}
        this.$emit('orderedBurgers', {name: this.burger.name, amount: this.amountOrdered});
      }
    }
  }

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .wrapper{
    display: grid;
    grid-template-columns: 326px 327px 326px;
    background-color: #fff;
    color: #444;

  }
  .burg{
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    font-size: 150%;
    grid-column: 1;
  }
  .allergy{
    font-weight: bold;
  }
  #pic_burgar{
    width: 250px;
    height: 250px;
  }
  </style>
  