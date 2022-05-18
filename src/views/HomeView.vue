<template>
  <div class="home">
    <navHeaderViewVue />
    <div class="contenaire">
      <h1>Bienvenue sur le réseau social de l'entreprise 👋</h1>
      <div class="card">
        <div class="connexion d-flex justify-content-between">
          <p>Pour créer un compte rendez sur la page</p>
          <router-link to="/signup">
            <button class="btn btn-outline-primary mr-2">
              Créer un compte
            </button>
          </router-link>
        </div>
        <div class="connexion d-flex justify-content-between">
          <p>Vous avez déjà un compte</p>
          <router-link to="/signin">
            <button class="btn btn-outline-primary mr-2">
              Connexion
            </button></router-link
          >
        </div>
      </div>
    </div>
    <footerView />
  </div>
</template>

<script>
// @ is an alias to /src

import navHeaderViewVue from "@/components/navHeaderView.vue";
import footerView from "@/components/footerView.vue";

export default {
  name: "HomeView",
  components: {
    navHeaderViewVue,
    footerView,
  },
};
</script>
<style scoped>
.contenaire {
  height: 70vh;
  margin: 0;
  padding: 20px;
  background-repeat: no-repeat;
  background-position: 0% 45%;
  background-image: url("../assets/social-bg-removebg-preview.png");
}
h1 {
  margin: 20px 15px;
}
.card {
  margin: auto;

  width: 500px;
  background-color: rgb(200 216 233);
  border-radius: 30px;

  justify-content: center;
  align-items: flex-start;
  padding: 15px;
}
.connexion {
  width: 100%;
  margin: 7px 0px;
}
</style>
