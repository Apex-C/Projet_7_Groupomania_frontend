<template>
  <div class="wall">
    <NavHeaderWallView />

    <div class="userInfo">
      <div class="border border-3 border-dark">
        <img class="imageInfo" :src="user.avatar" />
      </div>
      <p>{{ user.userName }}</p>
      <input
        type="input"
        class="justify-content-center col-10 rounded"
        data-toggle="modal"
        data-target="#exampleModal"
        placeholder="Créer un message"
      />
    </div>
    <NoMessageView v-if="messages.length === 0"></NoMessageView>

    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              Poster un nouveau message
            </h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="row modal-body">
            <form method="post" enctype="multipart/form-data">
              <div class="row modal-body">
                <div class="col-12 justify-content-center form-group">
                  <textarea
                    v-model="newMessage"
                    class="form-control"
                    id="newPost"
                    name="message"
                    rows="5"
                    placeholder="Entrez votre message..."
                  ></textarea>
                </div>
                <div
                  class="col-12 justify-content-center text-center image-box"
                >
                  <img :src="newImage" class="rounded new-image" />
                  <p class="small text-center">Image à partager</p>
                </div>
                <div class="col-12justify-content-center m-auto">
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
                    />
                  </div>
                </div>
              </div>
              <div class="modal-footer">
                <div class="row w-100 justify-content-spacebetween">
                  <div class="col-6">
                    <button
                      data-dismiss="modal"
                      class="btn btn-secondary btn-block"
                    >
                      Annuler
                    </button>
                  </div>
                  <div class="col-6">
                    <button
                      type="submit"
                      @click.prevent="addNewMessage()"
                      class="btn btn-success btn-block"
                      data-dismiss="modal"
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
    <div
      class="modal fade"
      id="updateModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              Modifier un nouveau message
            </h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="row modal-body">
            <form method="post" enctype="multipart/form-data">
              <div class="row modal-body">
                <div class="col-12 justify-content-center form-group">
                  <textarea
                    v-model="newMessage"
                    class="form-control"
                    id="newPost"
                    name="message"
                    rows="5"
                    placeholder="Entrez votre message..."
                  ></textarea>
                </div>
                <div
                  class="col-12 justify-content-center text-center image-box"
                >
                  <img :src="newImage" class="rounded new-image" />
                  <p class="small text-center">Image à partager</p>
                </div>
                <div class="col-12justify-content-center m-auto">
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
                    />
                  </div>
                </div>
              </div>
              <div class="modal-footer">
                <div class="row w-100 justify-content-spacebetween">
                  <div class="col-6">
                    <button
                      data-dismiss="modal"
                      class="btn btn-secondary btn-block"
                    >
                      Annuler
                    </button>
                  </div>
                  <div class="col-6">
                    <button
                      type="submit"
                      @click.prevent="addNewMessage()"
                      class="btn btn-success btn-block"
                      data-dismiss="modal"
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
    <div
      v-for="message in messages"
      :key="message.id"
      class="card bg-light my-3"
    >
      <div class="card-header bg-light">
        <img :src="message.avatar" width="40" class="m-0 rounded-circle" />
        <span class="small text-dark m-0 p-1">
          Posté par {{ message.userName }}
          , le
          {{
            message.createdAt.slice(0, 10).split("-").reverse().join("/") +
            " à " +
            message.createdAt.slice(11, 16)
          }}
        </span>
        <a
          @click="updateMessage(message.id)"
          data-toggle="modal"
          data-target="#updateModal"
          ><i class="fa-solid fa-pen"></i
        ></a>
        <a
          @click="deleteMessage(message.id)"
          v-if="message.UserId == user.id || isAdmin == true"
        >
          <i class="fa-solid fa-trash-can"></i>
        </a>
      </div>
      <div class="card-body text-dark text-left">
        <p class="txt_msg small" v-if="message.message !== ''">
          {{ message.message }}
        </p>
        <img
          class="message_image"
          :src="message.messageUrl"
          v-if="message.messageUrl !== ''"
        />
      </div>
      <hr class="m-0" />
      <div class="social">
        <svg
          @click="likeRules(message.id)"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
          height="30"
          id="like"
        >
          <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
          <path
            d="M96 191.1H32c-17.67 0-32 14.33-32 31.1v223.1c0 17.67 14.33 31.1 32 31.1h64c17.67 0 32-14.33 32-31.1V223.1C128 206.3 113.7 191.1 96 191.1zM512 227c0-36.89-30.05-66.92-66.97-66.92h-99.86C354.7 135.1 360 113.5 360 100.8c0-33.8-26.2-68.78-70.06-68.78c-46.61 0-59.36 32.44-69.61 58.5c-31.66 80.5-60.33 66.39-60.33 93.47c0 12.84 10.36 23.99 24.02 23.99c5.256 0 10.55-1.721 14.97-5.26c76.76-61.37 57.97-122.7 90.95-122.7c16.08 0 22.06 12.75 22.06 20.79c0 7.404-7.594 39.55-25.55 71.59c-2.046 3.646-3.066 7.686-3.066 11.72c0 13.92 11.43 23.1 24 23.1h137.6C455.5 208.1 464 216.6 464 227c0 9.809-7.766 18.03-17.67 18.71c-12.66 .8593-22.36 11.4-22.36 23.94c0 15.47 11.39 15.95 11.39 28.91c0 25.37-35.03 12.34-35.03 42.15c0 11.22 6.392 13.03 6.392 22.25c0 22.66-29.77 13.76-29.77 40.64c0 4.515 1.11 5.961 1.11 9.456c0 10.45-8.516 18.95-18.97 18.95h-52.53c-25.62 0-51.02-8.466-71.5-23.81l-36.66-27.51c-4.315-3.245-9.37-4.811-14.38-4.811c-13.85 0-24.03 11.38-24.03 24.04c0 7.287 3.312 14.42 9.596 19.13l36.67 27.52C235 468.1 270.6 480 306.6 480h52.53c35.33 0 64.36-27.49 66.8-62.2c17.77-12.23 28.83-32.51 28.83-54.83c0-3.046-.2187-6.107-.6406-9.122c17.84-12.15 29.28-32.58 29.28-55.28c0-5.311-.6406-10.54-1.875-15.64C499.9 270.1 512 250.2 512 227z"
          />
        </svg>
        <span> {{ like }} </span>
        <svg
          @click="disLikeRules(message.id)"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
          height="30"
          id="dislike"
        >
          <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
          <path
            d="M128 288V64.03c0-17.67-14.33-31.1-32-31.1H32c-17.67 0-32 14.33-32 31.1v223.1c0 17.67 14.33 31.1 32 31.1h64C113.7 320 128 305.7 128 288zM481.5 229.1c1.234-5.092 1.875-10.32 1.875-15.64c0-22.7-11.44-43.13-29.28-55.28c.4219-3.015 .6406-6.076 .6406-9.122c0-22.32-11.06-42.6-28.83-54.83c-2.438-34.71-31.47-62.2-66.8-62.2h-52.53c-35.94 0-71.55 11.87-100.3 33.41L169.6 92.93c-6.285 4.71-9.596 11.85-9.596 19.13c0 12.76 10.29 24.04 24.03 24.04c5.013 0 10.07-1.565 14.38-4.811l36.66-27.51c20.48-15.34 45.88-23.81 71.5-23.81h52.53c10.45 0 18.97 8.497 18.97 18.95c0 3.5-1.11 4.94-1.11 9.456c0 26.97 29.77 17.91 29.77 40.64c0 9.254-6.392 10.96-6.392 22.25c0 13.97 10.85 21.95 19.58 23.59c8.953 1.671 15.45 9.481 15.45 18.56c0 13.04-11.39 13.37-11.39 28.91c0 12.54 9.702 23.08 22.36 23.94C456.2 266.1 464 275.2 464 284.1c0 10.43-8.516 18.93-18.97 18.93H307.4c-12.44 0-24 10.02-24 23.1c0 4.038 1.02 8.078 3.066 11.72C304.4 371.7 312 403.8 312 411.2c0 8.044-5.984 20.79-22.06 20.79c-12.53 0-14.27-.9059-24.94-28.07c-24.75-62.91-61.74-99.9-80.98-99.9c-13.8 0-24.02 11.27-24.02 23.99c0 7.041 3.083 14.02 9.016 18.76C238.1 402 211.4 480 289.9 480C333.8 480 360 445 360 411.2c0-12.7-5.328-35.21-14.83-59.33h99.86C481.1 351.9 512 321.9 512 284.1C512 261.8 499.9 241 481.5 229.1z "
          />
        </svg>
        <span> {{ disLike }} </span>
      </div>
      <span> </span>
      <a @click="commentaire(message.id)"> Commentaires </a>

      <CommentairesViewVue
        v-if="comment && message.id == this.$route.params.id"
      ></CommentairesViewVue>
    </div>
  </div>
</template>

<script>
import NavHeaderWallView from "@/components/navHeaderWallView.vue";
import CommentairesViewVue from "@/components/CommentairesView.vue";

import NoMessageView from "@/components/NoMessageView.vue";
import axios from "axios";
//import router from "@/router";

export default {
  name: "wallView",
  components: {
    NavHeaderWallView,
    CommentairesViewVue,
    NoMessageView,
  },
  data() {
    return {
      isAdmin: false,
      currentUserId: "",

      isActive: true,
      newImage: "",
      newMessage: "",
      file: null,
      messages: [],
      user: [],
      comment: false,
      like: 0,
      disLike: 0,
    };
  },

  methods: {
    onFileChange() {
      this.file = this.$refs.file.files[0];
      this.newImage = URL.createObjectURL(this.file);
    },
    addNewMessage() {
      const formData = new FormData();
      formData.set("image", this.file);
      formData.set("UserId", this.currentUserId.toString());
      formData.set("message", this.newMessage.toString());
      axios
        .post("http://127.0.0.1:3000/api/messages/", formData, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })

        .then(() => {
          this.UserId = "";
          this.newMessage = "";
          this.file = null;
          document
            .querySelector(".btn.btn-success.btn-block")
            .setAttribute("data-dismiss", "modal");
          this.loadAllMessages();
        })

        .catch((err) => console.log(err));
    },
    commentaire(messageId) {
      console.log(messageId);
      this.$router.push("/wall/message/" + messageId);
      this.comment = !this.comment;
    },
    deleteMessage(messageId) {
      console.log(messageId);
      axios
        .delete("http://127.0.0.1:3000/api/messages/" + messageId, {
          headers: { Authorization: "Bearer " + localStorage.getItem("token") },
        })
        .then((res) => {
          if (res.status === 200) {
            //    location.reload();
            console.log("route");
            this.loadAllMessages();
          }
        });
    },
    loadAllMessages() {
      fetch("http://127.0.0.1:3000/api/messages/", {
        headers: { Authorization: "Bearer " + localStorage.getItem("token") },
      })
        .then((result) => result.json())
        .then((rest) => {
          this.messages = rest.allMessages;
          console.log(this.messages);
        })
        .catch((err) => {
          console.error(err);
        });
    },
    updateMessage(messageId) {
      const formData = new FormData();
      formData.set("image", this.file);
      formData.set("UserId", this.currentUserId.toString());
      formData.set("message", this.newMessage.toString());
      console.log(messageId);
      axios
        .put("http://127.0.0.1:3000/api/messages/" + messageId, formData, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then(() => {
          this.file = null;
          document
            .querySelector(".btn.btn-success.btn-block")
            .setAttribute("data-dismiss", "modal");
          this.loadAllMessages();
        })

        .catch((err) => console.log(err));
    },

    likeRules(messageId) {
      console.log(messageId);
      /**  this.like += 1;
       const likeElt = document.getElementById("like");
      if (this.like == 1) {
        likeElt.style.fill = "black";
        this.like--;
        return;
      }
      this.like++;
      likeElt.style.fill = "green";
      if (this.disLike == 1) this.disLike--; */
    },
    disLikeRules(messageId) {
      console.log(messageId);
      /**   this.disLike += 1;
       const likeElt = document.getElementById("dislike");
      if (this.disLike == 1) {
        likeElt.style.fill = "black";
        this.disLike--;

        return;
      }
      this.disLike++;
      likeElt.style.fill = "red";
      if (this.like == 1) this.like--; */
    },
  },
  created: function () {
    this.isAdmin = localStorage.getItem("role");
    this.currentUserId = localStorage.getItem("userId");
    if (localStorage.getItem("refresh") === null) {
      localStorage.setItem("refresh", 0);
      location.reload();
    }
    fetch(`http://127.0.0.1:3000/api/users/${this.currentUserId}`, {
      headers: { Authorization: "Bearer " + localStorage.getItem("token") },
    })
      .then((rest) => rest.json())
      .then((data) => {
        console.log(data);
        this.user = data;
      })
      .catch((err) => console.log(err));
    this.loadAllMessages();
  },
};
</script>

<style lang="scss" scoped>
.imageInfo {
  width: 80px;
}
.card.bg-light.my-3 {
  width: 480px;
  margin: auto;
}
.message_image {
  height: 250px;
}
.card-header.bg-light {
  text-align: left;
}
.txt_msg {
  text-align: left;
}
.userInfo {
  width: 65%;
  margin: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: antiquewhite;
  padding: 15px;
}
.card-body {
  background: snow;
}
svg#like,
svg#dislike {
  margin: 5px 10px;
}
.text_left {
  text-align: left;
}

.image-box .new-image {
  height: 150px;
}
.commentaire_comment {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
textarea.commentaire {
  flex: 1;
  margin: 10px;
}
img.m-0.rounded-circle {
  border: 2px black solid;
  height: 40px;
  width: 40px;
  object-fit: contain;
}
svg.svg-inline--fa.fa-trash-can {
  margin-left: 50px;
  &:hover {
    color: coral;
  }
}
.social {
  display: flex;
  align-items: center;
  justify-content: center;
}
svg.svg-inline--fa.fa-pen {
  margin-left: 20px;
  &:hover {
    color: green;
  }
}
</style>
