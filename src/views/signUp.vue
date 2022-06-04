<template>
  <div class="about">
    <NavHeaderView></NavHeaderView>

    <div class="contenaire">
      <div class="image-card">
        <img class="bg_img" src="../assets/Groupomania.jpg" alt="" />
      </div>
      <div class="card">
        <h1>Créer un compte</h1>
        <p>
          Vous avez déjà un compte
          <router-link to="/signin">Connexion</router-link>
        </p>
        <form @submit.prevent="">
          <div class="contenaire card_body">
            <label for="pseudo">pseudo</label>
            <input
              v-model="pseudo"
              @input="isActife"
              type="text"
              id="pseudo"
              placeholder=" pseudo"
              required
            />
            <label for="email">Email</label>
            <input
              v-model="email"
              @input="isActife"
              type="text"
              id="email"
              placeholder=" example@gmail.com"
              required
            />

            <label for="password">Mot de passe</label>
            <input
              v-model="password"
              @input="isActife"
              type="password"
              id="password"
              placeholder=" mot de passe"
              required
            />
          </div>
          <p id="connexion-error"></p>
          <button
            class="btn btn-primary"
            type="submit"
            value="Connexion"
            @click="createAcompte"
            :disabled="isDisabled"
          >
            Connexion
          </button>
        </form>
      </div>
    </div>
    <FooterView />
  </div>
</template>

<script>
import NavHeaderView from "@/components/navHeaderView.vue";
import axios from "axios";
import router from "@/router";
import FooterView from "../components/footerView.vue";

export default {
  name: "signUp",
  components: {
    NavHeaderView,
    FooterView,
  },
  data() {
    return {
      pseudo: "",
      email: "",
      password: "",
      isDisabled: true,
    };
  },
  methods: {
    createAcompte() {
      const userName = this.pseudo;
      const email = this.email;
      const password = this.password;

      axios
        .post("http://127.0.0.1:3000/api/auth/signup", {
          name: userName,
          email: email,
          password: password,
        })
        .then(function (response) {
          console.log(response);
          if (response.statusText === "Created") {
            console.log("ok");
            axios
              .post("http://127.0.0.1:3000/api/auth/login", {
                email: email,
                password: password,
              })

              .then((response) => {
                console.log(response);
                localStorage.setItem("token", response.data.token);
                localStorage.setItem("userId", response.data.userId);
                localStorage.setItem("userName", response.data.userName);
                localStorage.setItem("avatar", response.data.avatar);
                localStorage.setItem("role", response.data.role);
                router.push("/wall");
              });
          }
        })

        .catch(function (error) {
          const eltError = document.getElementById("connexion-error");
          const codeError = error.message.split("code ")[1];
          let messageError = "";
          switch (codeError) {
            case "401":
              console.log(error.response.data.error.errors[0].message);
              messageError = error.response.data.error.errors[0].message;
              break;
            case "402":
              console.log(error.response.data.error);
              messageError = error.response.data.error;
              break;
          }
          eltError.style.color = "red";
          eltError.innerHTML = messageError;
        });
    },
    isActife() {
      if (this.pseudo != "" && this.email != "" && this.password != "") {
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
  padding: 10px;
  border-radius: 30px;
  background-color: rgb(188 188 227);

  position: absolute;
  top: 25px;
  right: 20px;
}
.card_body {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 155px;
  justify-content: space-evenly;
  margin-bottom: 10px;
}
#connexion-error {
  font-size: 10px;
  width: 200px;
  color: red;
  padding: 10px;
  margin: 0px auto;
}
.btn {
  align-self: center;
  margin-top: 15px;
}
</style>
