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
        <div class="card_body">
          <label for="pseudo">pseudo</label>
          <input
            v-model="pseudo"
            @input="isActife"
            type="text"
            id="pseudo"
            placeholder=" pseudo"
          />
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
          @click="createAcompte"
          :disabled="isDisabled"
        >
          Connexion
        </button>
      </div>
    </div>
    <FooterView />
  </div>
</template>

<script>
import NavHeaderView from "@/components/navHeaderView.vue";
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

      fetch("http://127.0.0.1:3000/api/auth/signup", {
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name: userName,
          email: email,
          password: password,
        }),
      })
        .catch((err) => {
          console.log(err);
        })
        .then((res) => {
          if (res.ok) {
            console.log("tout va bien ");
            fetch("http://127.0.0.1:3000/api/auth/login", {
              method: "post",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify({
                email: email,
                password: password,
              }),
            })
              .then((rest) => rest.json())
              .then((response) => {
                localStorage.setItem("token", response.token);
                localStorage.setItem("userId", response.userId);
                localStorage.setItem("userName", response.userName);
                localStorage.setItem("avatar", response.avatar);
                localStorage.setItem("role", response.role);
                router.push("/wall");
              })
              .catch((error) => {
                console.log(error);
                document.getElementById("connexion-error").innerHTML =
                  error.response.data.error;
              });
          }
        })
        .catch((err) => {
          console.log(err);
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
  top: 5px;
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
.btn {
  align-self: center;
  margin-top: 15px;
}
</style>
