<template>

<div class="todo">
  <div v-if="restaurant">

    <br>
    <br>

    <h1>Détails du restaurant qui a pour id : {{ id }}</h1>
    <ul class='div_presentation'>
      <br>
      <br>

      <li>Nom : {{ restaurant.name }}</li>
      <li>Cuisine : {{ restaurant.cuisine }}</li>
      <li>Ville : {{ restaurant.borough }}</li>

      <br>
      
    </ul>
    <Map :coords="restaurant.address.coord"></Map>

    <br>
    <br>

  <div class="photo">
        
        <img src="../.././imgProject.jpeg" >

    </div>

  </div>

</div>
</template>

<script>
import Map from "./Map.vue";

export default {
  name: "Restaurant",
  props: {},
  components: {
    Map,
  },
  computed: {
    id() {
      return this.$route.params.id;
    },
  },

  data: function () {
    return {
      restaurant: null,
    };
  },
  mounted() {
    console.log("Avant affichae,on pourra faire un fetch...");
    console.log("ID = " + this.id);
    let url = "http://localhost:8080/api/restaurants/" + this.id;

    fetch(url)
      .then((reponse) => {
        return reponse.json();
      })
      .then((data) => {
        console.log(data.restaurant.name);
        this.restaurant = data.restaurant;
        console.log(this.restaurant);
      });
  },
  methods: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

ul.div_presentation{

  margin: auto;
  width: 20%;
  padding: 20px;


}

h1{

  text-align: center;
  color: tan;
}

li{

  size: 50px;
}

.photo{

 text-align: center;
  color: tan;
  
}

.todo{

  background-color: beige;
    tab-size: 1OOpx;

}




</style>
