<template>
  <div class="ListeResto">
    <div class="page-top">
      <h2>{{ msg }}</h2>

      <h1>Nombre de restaurants : {{ nbdeRestaurantsTotal }}</h1>

      <p>nbdepageTotal : {{ nbdepageTotal }}</p>
      <p>
        nombre de restaurant par page:
        <input
          @change="getRestaurantsFromServeur()"
          type="range"
          min="2"
          max="100"
          v-model="pagesize"
        />{{ pagesize }}
      </p>
    </div>

    <md-table
      v-model="restaurants"
      md-sort="name"
      md-sort-order="asc"
      md-card
      md-fixed-header
    >
      <md-table-toolbar>
        <div class="md-toolbar-section-start">
          <h1 class="md-title">Restaurants</h1>
        </div>
        <AddRestaurantDialog></AddRestaurantDialog>

        <md-field md-clearable class="md-toolbar-section-end">
          <md-input
            placeholder="Rechercher par nom..."
            v-model="nomRestauRecherche"
            @input="ChercherRestaurant"
          />
        </md-field>
      </md-table-toolbar>
      <md-table-row
        slot="md-table-row"
        slot-scope="{ item, index }"
        :style="{ backgroundColor: getColor(index) }"
        :class="{ bordureRouge: index === 2 }"
      >
        <md-table-cell md-label="Nom" md-sort-by="name">{{
          item.name
        }}</md-table-cell>

        <md-table-cell md-label="Cuisine" md-sort-by="cuisine">{{
          item.cuisine
        }}</md-table-cell>

        <md-table-cell md-label="Ville" md-sort-by="borough">{{
          item.borough
        }}</md-table-cell>

        <md-table-cell md-label="Action">
          <div v-if="nbdeRestaurantsTotal">
                      <router-link :to="'/restaurant/' + item._id"
            ><md-button class="md-secondary md-raised" matTooltip="Details"
              >Détails</md-button
            ></router-link
          >

          <md-button
            class="md-icon-button md-list-action"
            @click="supprimerRestaurant(item)"
            matTooltip="Delete task"
          >
            <md-tooltip md-direction="top">Delete task</md-tooltip>
            <md-icon>delete</md-icon>
          </md-button>
          </div>

        </md-table-cell>
      </md-table-row>
    </md-table>

    <md-button
      class="md-raised"
      :disabled="page === 0"
      @click="pagePrécédent()"
    >
      Précédent</md-button
    >
    <md-button
      class="md-raised"
      :disabled="page === nbdepageTotal"
      @click="pageSuivant()"
    >
      Suivant
    </md-button>
    <p>
      <strong> Page courante - {{ page }} </strong>
    </p>
  </div>
</template>

<script>
import _ from "lodash";

import AddRestaurantDialog from "./AddRestaurantDialog.vue";

export default {
  name: "ListeDesRestaurants",
  components: {
    AddRestaurantDialog,
  },
  data: function () {
    return {
      restaurants: [],
      ville: "",
      nbdeRestaurantsTotal: 0,
      page: 0,
      pagesize: 10,
      nbdepageTotal: 0,
      msg: "",
      nomRestauRecherche: "",
    };
  },
  mounted() {
    console.log("AVANT.RENDU.HTML");
    this.getRestaurantsFromServeur();
  },
  methods: {
    pagePrécédent() {
      if (this.page === 0) return;
      this.page--;
      this.getRestaurantsFromServeur();
    },
    pageSuivant() {
      if (this.page === this.dernierePage) return;
      this.page++;
      this.getRestaurantsFromServeur();
    },

    getRestaurantsFromServeur() {
      let url = "http://localhost:8080/api/restaurants?";
      let Url1 = url + "page=" + this.page;
      Url1 += "&pagesize=" + this.pagesize;
      Url1 += "&name=" + this.nomRestauRecherche;

      fetch(Url1)
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            this.restaurants = resJS.data;
            this.nbdeRestaurantsTotal = resJS.count;
            this.nbdepageTotal = Math.round(
              this.nbdeRestaurantsTotal / this.pagesize
            );
            console.log(this.restaurants)

            if (this.nbdeRestaurantsTotal == 0) {
              this.restaurants = [{name: "Aucun restaurant ne correspond à votre recherche"}]
            }
          });
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    ChercherRestaurant: _.debounce(function () {
      
      this.getRestaurantsFromServeur();
    }, 300),


    supprimerRestaurant(r) {
      console.log("WE ARE HERE");
      let url = "http://localhost:8080/api/restaurants/" + r._id;
      fetch(url, {
        method: "DELETE",
      })
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            console.log(resJS.msg);
            this.msg = resJS.msg;
            this.getRestaurantsFromServeur();
          });
        })
        .catch(function (err) {
          console.log(err);
        });
    },

    getColor(index) {
      return index % 2 ? "#EFEFEF" : "white";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.md-table-cell:last-child .md-table-cell-container,
.md-table-head:last-child {
  text-align: center !important;
  display: flex;
  justify-content: center;
}
.page-top {
  text-align: center;
  margin-bottom: 60px;
  background-color: beige;
}

.ListeResto{

  background-color: beige;
}
</style>