<template>
  <div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-title>Ajouter un restaurant</md-dialog-title>
      <form
        novalidate
        class="md-layout centered-container"
        @submit.prevent="ajouterRestaurant"
      >
        <md-dialog-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="nom">Nom du restaurant</label>
                <md-input name="nom" id="nom" required v-model="nom" />
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="ville">Ville du restaurant</label>
                <md-input name="ville" id="ville" required v-model="ville" />
              </md-field>
            </div>
          </div>

          <md-field>
            <label for="cuisine">Cuisine</label>
            <md-input name="cuisine" id="cuisine" required v-model="cuisine" />
          </md-field>
          <md-dialog-actions>
            <md-button class="md-primary" @click="showDialog = false"
              >Close</md-button
            >
            <md-button type="submit" class="md-primary"
              >Ajouter le restaurant</md-button
            >
          </md-dialog-actions>
        </md-dialog-content>
      </form>
    </md-dialog>

    <div class="button-add">
      <md-button class="md-primary md-raised" @click="showDialog = true"
        ><md-icon>add</md-icon> Ajouter</md-button
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "AddRestaurantDialog",
  data: () => ({
    nom: "",
    ville: "",
    cuisine: "",
    showDialog: false,
  }),
  methods: {
    ajouterRestaurant(event) {
      // Récupération du formulaire. Pas besoin de document.querySelector
      // ou document.getElementById puisque c'est le formulaire qui a généré
      // l'événement
      let form = event.target;

      // Récupération des valeurs des champs du formulaire
      // en prévision d'un envoi multipart en ajax/fetch
      let donneesFormulaire = new FormData(form);

      let url = "http://localhost:8080/api/restaurants";

      fetch(url, {
        method: "POST",
        body: donneesFormulaire,
      })
        .then((responseJSON) => {
          responseJSON.json().then((resJS) => {
            // Maintenant res est un vrai objet JavaScript
            console.log(resJS.msg);
            this.msg = resJS.msg;
          });
        })
        .catch((err) => {
          console.log(err);
        });

      this.nom = "";
      this.cuisine = "";
      this.showDialog = false;
    },
  },
};
</script>

<style scoped>
.button-add {
  margin-right: 16px;
}
</style>