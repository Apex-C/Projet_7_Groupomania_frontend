<template>
  <div class="about">
    <NavHeaderView></NavHeaderView>

    <div class="contenaire">
      <div class="image-card">
        <img class="bg_img" src="../assets/Groupomania.jpg" alt="" />
      </div>
    </div>
    <div class="card">
      <h1>Connexion</h1>
      <p>
        Pas encore de compte
        <router-link to="/signup">Créer un compte</router-link>
      </p>
      <div class="card_body">
        <label for="email">Email</label>
        <input
          v-model="email"
          @input="isActife"
          type="text"
          id="email"
          placeholder=" example@gmail.com"
        />

        <label for="password">Mot de passe</label>
        <input
          v-model="password"
          @input="isActife"
          type="text"
          id="password"
          placeholder=" mot de passe"
        />
      </div>
      <p id="connexion-error"></p>
      <button
        class="btn btn-primary"
        type="submit"
        value="Connexion"
        :disabled="isDisabled"
        @click="loginToAccompte"
      >
        Connexion
      </button>
    </div>

    <FooterView />
  </div>
</template>

<script>
import axios from "axios";
import NavHeaderView from "@/components/navHeaderView.vue";
import router from "@/router";
import FooterView from "@/components/footerView.vue";

export default {
  name: "signIn",

  components: {
    NavHeaderView,
    FooterView,
  },
  data() {
    return {
      email: "",
      password: "",
      isDisabled: true,
      curentUser: [],
    };
  },
  methods: {
    loginToAccompte() {
      this.submitted = true;

      axios
        .post("http://127.0.0.1:3000/api/auth/login", {
          email: this.email,
          password: this.password,
        })
        .then(function (response) {
          console.log(response.data);
          localStorage.setItem("token", response.data.token);
          localStorage.setItem("userId", response.data.userId);
          localStorage.setItem("userName", response.data.userName);
          localStorage.setItem("avatar", response.data.avatar);
          localStorage.setItem("role", response.data.role);
          router.push("/wall");
        })
        .catch(function (error) {
          document.getElementById("connexion-error").innerHTML =
            error.response.data.error;
        });
    },
    isActife() {
      if (this.email != "" && this.password != "") {
        return (this.isDisabled = false);
      }
      return (this.isDisabled = true);
    },
  },
};
</script>
<style scoped>
.contenaire {
  position: relative;
}

.image-card {
  width: 100%;
  height: 80vh;
}
.bg_img {
  height: inherit;
  float: left;
  object-fit: cover;
}
.card {
  padding: 15px;
  border-radius: 30px;
  background-color: rgb(188 188 227);
  width: fit-content;
  margin: auto;
  position: absolute;
  top: 70px;
  right: 20px;
}
.card_body {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 125px;
  justify-content: space-evenly;
}
.btn {
  align-self: center;
  margin-top: 10px;
}
#connexion-error {
  color: red;
}
</style>
