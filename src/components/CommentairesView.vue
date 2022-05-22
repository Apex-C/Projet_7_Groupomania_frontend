<template>
  <div>
    <div class="add-comment">
      <p class="text-left">Ajouter un commentaire</p>
      <form @submit.prevent="addComment()" class="form-comment">
        <textarea
          class="text-comment"
          v-model="newComment"
          placeholder="Entrez votre commentaire ici ..."
        ></textarea>
        <button
          class="btn_commentaire btn btn-success"
          type="submit"
          value="Submit"
        >
          Envoyer
        </button>
      </form>
    </div>
    <div
      class="modal fade"
      id="updateCommentModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="updateCommentModal">
              Modifier un commentaire
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
            <form method="post" enctype="multipart/form-data" class="w-100">
              <div class="row modal-body">
                <div class="col-12 justify-content-center form-group">
                  <textarea
                    v-model="newComment"
                    class="form-control"
                    id="newPost"
                    name="message"
                    placeholder="Entrez votre message..."
                  ></textarea>
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
                      @click.prevent="updateComment()"
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

    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 col-md-10 col-lg-8"></div>
        <div class="col-12 col-md-10 col-lg-8" id="OneMessage">
          <div class="card-card-comments" v-if="hasComments">
            <div class="card-comments">
              <div class="comments" v-for="item in oneMessage" :key="item.id">
                <div class="comments-head d-flex">
                  <img
                    :src="item.User.avatar"
                    width="30"
                    class="m-0 rounded-circle p-1"
                  />

                  <p class="font-weight-bold align-self-center text-nom">
                    poster par {{ item.User.userName }}, le
                    {{
                      item.createdAt.slice(0, 10).split("-").reverse().join("/")
                    }}
                  </p>

                  <a
                    v-if="item.UserId == currentUserId || isAdmin == true"
                    data-toggle="modal"
                    data-target="#updateCommentModal"
                    ><i class="fa-solid fa-pen"></i
                  ></a>

                  <a
                    @click="deleteComment()"
                    v-if="item.UserId == currentUserId || isAdmin"
                  >
                    <span :class="{ active: isActive }">
                      {{ (this.commentID = item.id) }}
                    </span>
                    <i class="fa-solid fa-trash-can"></i>
                  </a>
                </div>
                <p class="comment-box">{{ item.comment }}</p>
              </div>
            </div>
          </div>
        </div>

        <div class="col-12 col-md-10 col-lg-12">
          <div
            v-for="comment in comments"
            :key="comment.id"
            class="card bg-light my-3"
          >
            <div class="card-header align-items-center m-0 p-1">
              <div class="d-flex justify-content-between">
                <span class="small text-dark m-0 p-1">
                  Commentaire de {{ comment.User.userName }}
                  , le
                  {{
                    comment.createdAt
                      .slice(0, 10)
                      .split("-")
                      .reverse()
                      .join("/")
                  }}
                </span>
              </div>
              <hr class="m-0 p-0 bg-secondary" />
              <p class="small text-dark m-0 p-1">{{ comment.comment }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
//import axios from "axios";
export default {
  name: "Commentaires-view",
  data() {
    return {
      newComment: "",
      currentUserId: "",
      submitted: false,
      isAdmin: false,
      isActive: true,
      oneMessage: [],
      hasComments: false,
      comment: false,
      commentID: "",
    };
  },
  methods: {
    loadAllComment() {
      fetch(
        `http://127.0.0.1:3000/api/comments/message/${this.$route.params.id}`,
        {
          headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        }
      )
        .then((data) => data.json())
        .then((res) => {
          this.oneMessage = res;
          if (this.oneMessage.length != 0) {
            this.hasComments = true;
          }
          console.log(this.oneMessage.length);
          for (let item of res) {
            console.log(item);
          }
        })
        .catch((err) => console.log(err));
    },
    addComment() {
      this.submitted = true;

      this.currentUserId = localStorage.getItem("userId");
      fetch("http://127.0.0.1:3000/api/comments/", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
        body: JSON.stringify({
          MessageId: this.$route.params.id,
          UserId: this.currentUserId,
          comment: this.newComment,
        }),
      });
      this.loadAllComment();
      this.newComment = "";
    },
    deleteComment() {
      fetch(`http://127.0.0.1:3000/api/comments/${this.commentID}`, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      }).then(() => this.loadAllComment());
      this.comment = true;
    },
    updateComment() {
      fetch(`http://127.0.0.1:3000/api/comments/${this.commentID}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
        body: JSON.stringify({
          comment: this.newComment,
        }),
      })
        .then(() => {
          this.newComment = "";
          this.loadAllComment();
        })
        .catch((err) => console.log(err));
    },
  },

  created: function () {
    this.loadAllComment();
    this.currentUserId = localStorage.getItem("userId");
  },
};
</script>

<style scoped lang="scss">
.form-comment {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.text-comment {
  flex: 0.9;
  border-radius: 15px;
  padding: 5px;
}

.comments {
  display: flex;
  flex-direction: column;
  border: 2px solid;
  border-radius: 15px;
  margin: 10px 5px;
  background-color: rgb(214, 213, 211);
  padding: 2px;
}
.comment-box {
  background-color: #fffdfb;
  width: 100%;
  text-align: left;
  margin: 0px;
  padding-left: 20px;
  border-radius: 0px 0px 15px 15px;
}
.text-nom {
  margin: 0px 10px 0px 10px;
}
svg.svg-inline--fa.fa-trash-can {
  align-self: center;

  margin-left: 25px;
  &:hover {
    color: crimson;
  }
}

.active {
  display: none;
}
</style>
