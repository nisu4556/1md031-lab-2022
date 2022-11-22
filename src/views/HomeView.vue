<template>
  <div>
    <header>
      <img id="headpic" src="https://assets-us-01.kc-usercontent.com/0542d611-b6d8-4320-a4f4-35ac5cbf43a6/2ecf24fe-9f8f-498c-800e-0daccc75a7c1/bar-insurance-header.jpg?w=1110&h=400&fit=crop">
      <H1 class="rub">Welcome to BurgerOnline </H1>
    </header>
    <main>
      <section id="burgers">
        <h3 class="rub2">Select burger</h3>
        <p class="txt">This is where you execute burger selection</p>
        <div class="wrapper" >
            <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurgers="addToOrder($event)">
            </Burger>
        </div>
      </section>
      <section id="contact">
        <h3 class="rub2">Customer information</h3>
        <p class=" txt">Necessary information</p>
        <h3 class="rub2">Delivery information:</h3>
        <form>

          <p>
          <label for="name">Name</label><br>
          <input type="text" id="firstname" v-model="name" required="required" placeholder="Kalle Karlsson"><br>

          <label for="Email">Email</label><br>
          <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address"><br>

          </p>

          <label for="payment">Payment</label><br>
          <select id="payment" v-model="payment" >
            <option value="card">Card</option>
            <option value="swish">Swish</option>
            <option value="klarna">Klarna </option>
            <option value="paypal">PayPal</option>
          </select><br>

          <p> Gender
            <input type="radio" id="male" v-model="gender" value="male">
            <label for="male">Male</label><br>
            <input type="radio" id="female" v-model="gender" value="female" >
            <label for="female">Female</label><br>
            <input type="radio" id="nan" v-model="gender" value="nan">
            <label for="nan">Do not wish to tell</label>
          </p>

        </form>

        <div id="scroll">
          <p>Please indicate point of delivery:</p>
          <div id="map" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }"
               v-on:click="setLocation"></div>
          <div id=loc v-bind:style="{ left: location.x + 'px',
                      top: location.y + 'px' }">
            T
          </div>
        </div>
        <div id="orders">
          <button type="submit" v-on:click="submitter">
            <img src="https://www.google.com/url?sa=i&url=https%3A%2F%2Fse.depositphotos.com%2Fvector-images%2Fbudb%25C3%25A4rare.html&psig=AOvVaw1cHZH4qDIKVMg3fgHTr-zb&ust=1668605233498000&source=images&cd=vfe&ved=0CBAQjRxqFwoTCIC95YOlsPsCFQAAAAAdAAAAABAE"
               alt="" width="20px" height="20px">
            Send Info
          </button>
        </div>
      </section>

    </main>
    <footer class="foot">
      <p>&copy; 2022 Hypothetical burgers inc.</p>
    </footer>

  </div>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

function MenuItem(name, img, kCal, gluten, lactose){
  this.name = name;
  this.img = img;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}
const burgers = [new MenuItem("Kingburger", "https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Cheeseburger.jpg/1280px-Cheeseburger.jpg", 400, true, true),
  new MenuItem("Queenburger", "https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Cheeseburger.jpg/1280px-Cheeseburger.jpg", 350, true, false ),
  new MenuItem("Chiliburger", "https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Cheeseburger.jpg/1280px-Cheeseburger.jpg", 500, false, false )];

const socket = io();

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      gender: "nan",
      name: "",
      email: "",
      payment:"klarna",
      orderedBurgers:{},
      location:{x:0, y:0},
      burgers: menu
    }
  },
  methods: {
    setLocation: function(event){
      const offset = event.target.getBoundingClientRect();
      this.location.x = event.clientX-offset.left-10;
      this.location.y = event.clientY-offset.top-10;
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    submitter: function (){
      console.log(this.name, this.payment, this.email, this.gender, this.orderedBurgers)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
            details: { x: this.location.x, y: this.location.y  },
            orderItems: this.orderedBurgers,
            personalInfo:{  namn:this.name,
                            email:this.email,
                            gender:this.gender,
                            payment:this.payment}

          });

     // console.log(this.name, this.email, this.gender, this.street, this.HouseNR, this.payment ,this.orderedBurgers)
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },



    }
  }

</script>

<style>
#loc {
  background-color: black;
  color: white;
  text-align: center;
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 10px;
}
#scroll{
  overflow:scroll;
  width: 750px;
  height: 400px;
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}
  #map {
    width: 1920px;
    height: 1078px;
  }
  header{
    width: 100%;
    height: 200px;
    overflow:hidden;
  }
  .rub{
    position: absolute;
    margin-top: -370px;
  }
  #headpic{
    opacity: 50%;
  }
  body {
    width: 1000px;
    margin: 10px;
  }
  div{
    border: 5px blue;
  }
  #burgers{
    margin: 5px;
    border: 2px dashed grey;
    background:black;
    color: white;
  }
  .allergy{
    font-weight: bold;
  }
  #contact{
    margin: 5px;
    border:2px Dashed grey;

  }
  .wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 321px 322px 321px;
    background-color: #fff;
    color: #444;
  }
  .burg{
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    font-size: 150%;
  }


  #pic_burgar{
    width: 250px;
    height: 250px;
  }
  button:hover {
    background: green;
  }
</style>