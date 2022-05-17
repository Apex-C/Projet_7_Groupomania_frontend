<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 col-md-10 col-lg-8">
          <div class="card bg-light">
            <div
              class="card-header text-center"
              style="background-color: ghostwhite"
            >
              <span class="text-dark font-weight-bold">{{ userName }}</span>
            </div>
            <div class="card-body" style="background-color: #d4d4d4">
              <div
                class="row border d-flex align-items-center m-2 p-0"
                style="background-color: ghostwhite"
              >
                <div class="col-12 col-md-4 text-center">
                  <img :src="avatar" class="image_user rounded-circle" />
                  <a
                    href=""
                    class="btn btn-sm btn-primary mb-2 p-1"
                    data-toggle="modal"
                    data-target="#modalAvatar"
                    >Changer de photo</a
                  >
                </div>
                <div
                  class="modal fade"
                  id="modalAvatar"
                  tabindex="-1"
                  aria-labelledby="modalAvatar"
                  aria-hidden="true"
                >
                  <div class="modal-dialog">
                    <div class="modal-content">
                      <form
                        @submit.prevent="updateAvatar()"
                        enctype="multipart/form-data"
                      >
                        <div class="modal-header">
                          <p class="modal-title h5">
                            Changer la photo de profile
                          </p>
                        </div>
                        <div class="row modal-body">
                          <div class="col-6 justify-content-center">
                            <img :src="avatar" class="w-100 rounded-circle" />
                            <p class="small text-center">Photo actuelle</p>
                          </div>
                          <div class="col-6 justify-content-center">
                            <img
                              :src="newAvatar"
                              class="w-100 rounded-circle"
                            />
                            <p class="small text-center">Nouvelle photo</p>
                          </div>
                          <div class="col-12 justify-content-center">
                            <div class="form-group justify-content-center">
                              <label for="File" class="sr-only"
                                >Choisir une nouvelle photo</label
                              >
                              <input
                                @change="onFileChange()"
                                type="file"
                                ref="file"
                                name="image"
                                class="form-control-file"
                                id="File"
                                accept=".jpg, .jpeg, .gif, .png"
                                :class="{ 'is-invalid': submitted && !file }"
                              />
                              <div
                                v-show="submitted && !file"
                                class="invalid-feedback"
                              >
                                Une nouvelle photo est requise !
                              </div>
                            </div>
                          </div>
                        </div>
                        <div class="modal-footer">
                          <div class="row w-100 justify-content-spacebetween">
                            <div class="col-6">
                              <a
                                data-dismiss="modal"
                                class="btn btn-sm btn-secondary btn-block"
                                >Annuler</a
                              >
                            </div>
                            <div class="col-6">
                              <button
                                type="submit"
                                class="btn btn-sm btn-success btn-block"
                              >
                                Valider
                              </button>
                            </div>
                          </div>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
                <div class="col-12 col-md-8 text-dark">
                  <p class="small text-left m-0 p-1">
                    Adresse e-mail : {{ email }}
                  </p>
                  <p class="small text-left m-0 p-1">Rôle : {{ role }}</p>
                  <p class="small text-left m-0 p-1">
                    Crée le : {{ createdAt }}
                  </p>
                  <p class="small text-left m-0 p-1">Votre contenu :</p>
                  <ul>
                    <li class="small text-left m-0 p-1">
                      <a href="#/compte/messages"
                        >{{ messagesCount }} message<span
                          v-if="messagesCount > 1"
                          >s</span
                        ></a
                      >
                    </li>
                    <li class="small text-left m-0 p-1">
                      {{ commentsCount }} commentaire<span
                        v-if="commentsCount > 1"
                        >s</span
                      >
                    </li>
                  </ul>
                </div>
              </div>
            </div>
            <div class="card-footer" style="background-color: ghostwhite">
              <div>
                <div class="col-12 footer-btn">
                  <router-link
                    to="/wall"
                    class="my-2 btn btn-sm btn-block btn-success"
                  >
                    retour aux messages</router-link
                  >
                  <a
                    href=""
                    data-toggle="modal"
                    data-target="#modalDeleteAccount"
                    class="btn btn-sm btn-block btn-danger"
                    >Supprimer mon compte !</a
                  >
                </div>

                <div
                  class="modal fade"
                  id="modalDeleteAccount"
                  data-backdrop="static"
                  data-keyboard="false"
                  tabindex="-1"
                  aria-labelledby="modalDeleteAccount"
                  aria-hidden="true"
                >
                  <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content border border-danger">
                      <form enctype="multipart/form-data">
                        <div class="modal-header">
                          <p class="modal-title h5 text-danger">
                            Supprimer mon compte ?
                          </p>
                        </div>
                        <div class="row modal-body">
                          <div class="col-12 justify-content-center form-group">
                            <p class="text-danger">
                              <span class="h6">ATTENTION !</span> Cette action
                              est irreversible.
                            </p>
                            <p>
                              Vous ne pourrais plus vous connecter avec cette
                              adresse email mais vos messages et commentaires
                              seront toujours visible par les autres
                              utilisateurs.
                            </p>
                            <p>
                              Êtes-vous
                              <span class="font-weight-bold">certain</span> de
                              vouloir supprimer votre compte ?
                            </p>
                          </div>
                        </div>
                        <div class="modal-footer">
                          <div class="row w-100 justify-content-spacebetween">
                            <div class="col-6">
                              <a
                                data-dismiss="modal"
                                class="btn btn-secondary btn-block"
                                >Annuler</a
                              >
                            </div>

                            <div class="col-6">
                              <button
                                type="submit"
                                @click.prevent="deleteAccount()"
                                class="btn btn-danger btn-block"
                              >
                                Valider
                              </button>
                            </div>
                          </div>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CompteView",
  data() {
    return {
      userName: "",
      email: "",
      role: "",
      createdAt: "",
      messagesCount: "",
      commentsCount: "",
      avatar: "",
      newAvatar: "",
      file: null,
      submitted: false,
    };
  },
  methods: {
    onFileChange() {
      this.file = this.$refs.file.files[0];
      this.newImage = URL.createObjectURL(this.file);
      console.log(this.file);
    },
    deleteAccount() {
      console.log("suprimer");
    },
    updateAvatar() {
      console.log("suprimer");
    },
  },
  created: function () {
    const id = localStorage.getItem("userId");
    fetch(`http://127.0.0.1:3000/api/users/${id}`, {
      headers: { Authorization: "Beare" + localStorage.getItem("token") },
    })
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        this.userName = data.userName;
        this.email = data.email;
        this.createdAt = data.createdAt
          .slice(0, 10)
          .split("-")
          .reverse()
          .join();
        this.role = data.role;
        this.avatar = data.avatar;
        this.messagesCount = data.messagesCount;
        this.commentsCount = data.commentsCount;
      });
  },
};
</script>
<style scoped>
.image_user {
  height: 110px;
  margin: 15px 15px;
}
.footer-btn {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>
