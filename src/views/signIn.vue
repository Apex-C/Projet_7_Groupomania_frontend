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
        <p id="email-error"></p>
        <label for="password">Mot de passe</label>
        <input
          v-model="password"
          @input="isActife"
          type="text"
          id="password"
          placeholder=" mot de passe"
        />
      </div>
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
import NavHeaderView from "@/components/navHeaderView.vue";
//import router from "@/router";
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
      if (this.email != "" && this.password != "") {
        fetch(`http://127.0.0.1:3000/api/auth/login`, {
          method: "post",
          headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },

          body: JSON.stringify({
            email: this.email,
            password: this.password,
          }),
        })
          .then((result) => result.json())
          .then((data) => {
            console.log(data);
            this.curentUser = data;
            localStorage.setItem("token", data.token);
            localStorage.setItem("userId", data.userId);
            localStorage.setItem("userName", data.userName);
            localStorage.setItem("role", data.role);
            localStorage.setItem("avatar", data.avatar);

            // router.push("/wall");
          })
          .catch(function (error) {
            console.log("une erreur", error);
            document.getElementById("email-error").innerHTML = error.email;
          });
      }
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
  height: 320px;
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
  margin-top: 15px;
}
</style>
