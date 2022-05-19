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
                    @click="deleteComment()"
                    :href="`#/wall/message/comment/` + item.id"
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
        <div class="col-12 col-md-10 col-lg-8">
          <div
            class="modal fade"
            id="modalAddComment"
            tabindex="-1"
            aria-labelledby="modalAddComment"
            aria-hidden="true"
          ></div>
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
                  <span v-if="!comment.User.isActive" class="small text-danger"
                    >(supprimé)</span
                  >, le
                  {{
                    comment.createdAt
                      .slice(0, 10)
                      .split("-")
                      .reverse()
                      .join("/")
                  }}
                </span>
                <div
                  :id="'adcom' + comment.id"
                  v-if="
                    comment.UserId == this.currentUserId ||
                    this.isAdmin == 'true'
                  "
                >
                  <a :href="'#/commentaire/edit/' + comment.id"
                    ><img
                      src="/images/edit.svg"
                      class="m-1 p-0"
                      alt="Editer le message"
                      title="Editer le message"
                  /></a>
                  <a :href="'#/commentaire/drop/' + comment.id"
                    ><img
                      src="/images/drop.svg"
                      class="m-1 p-0"
                      alt="Supprimer le message"
                      title="Supprimer le message"
                  /></a>
                </div>
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
      comments: [],
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
  },

  created: function () {
    this.loadAllComment();
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
