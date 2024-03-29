<template>
  <div class="word-data-container" v-if="word">
    <h1>
      Search: <span>{{ word }}</span>
    </h1>
    <div class="outer">
      <div class="inner">    
        <p id="word-updated">{{ updatedWordNotification }}</p>
        <p class="author">
        <img src="@/assets/iconpacks-person.svg" alt="Author" />
        {{ author }}
        </p>
        <p id="word-definition">{{ definition }}</p>
      </div>
    </div>
    <h4 id="header-comments">Comments ({{ commentsTotal }})</h4>
    <Comment
      @votedComment="updateCommentVotes"
      @approvedComment="updateWordDefinition"
      @deletedComment="updateWordComments"
      v-for="comment in comments"
      :key="comment.id"
      :comment="comment"
    />
    <form class="comment-word-form" @submit.prevent="postComment">
      <label for="comments"
        >Suggest a more clear definition or more simple explanation for this
        word</label
      >
      <textarea
        id="comments"
        v-model="textareaInputValue"
        rows="5"
        placeholder="Share your thought..."
      />
      <input type="submit" value="Post" />
    </form>
  </div>
</template>

<script>
import Comment from "@/components/Comment";
export default {
  name: "WordInfo",

  components: {
    Comment,
  },

  data() {
    return {
      updatedWordNotification: null,
      textareaInputValue: null,
    };
  },

  computed: {
    word() {
      return this.$store.state.word;
    },
    definition() {
      return this.$store.state.definition;
    },
    author() {
      return this.$store.state.author;
    },
    comments() {
      return this.$store.state.comments;
    },
    commentsTotal() {
      let total;
      let comments = this.$store.state.comments;
      if (comments != undefined) {
        total = comments.length;
      }
      return total;
    },
  },

  methods: {
    updateWordDefinition(payload) {
        setTimeout(() => {
          this.$store.dispatch("getWord", payload.word);
          this.updatedWordNotification = "Word definition updated";
          document.getElementById("word-updated").style.color = "#42b983";
          document.body.scrollTop = 0; // For Safari
          document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE & Opera
        }, 700);
    },
    updateWordComments(word) {
        setTimeout(() => {
          this.$store.dispatch("getComments", word);
        }, 700);
    },
    updateCommentVotes(comment) {
      setTimeout(() => {
        this.$store.dispatch("getComments", comment.word);
      }, 700);
      let votesCounter = document.getElementById(`comment-votes-${comment.id}`);
      votesCounter.style.width = "90%";
      votesCounter.style.backgroundColor = "#1ac61a";
      votesCounter.style.color = "#fff"
      votesCounter.style.borderRadius = "6px";
    },
    postComment() {
      const firstname = this.$store.state.userService.user.firstname;
      const lastname = this.$store.state.userService.user.lastname;
      const email = this.$store.state.userService.user.email;
      this.$store.dispatch("postComment", {
        comment: this.textareaInputValue,
        word: this.word,
        author: `${firstname} ${lastname}`,
        email: email
      });
      setTimeout(() => {
        this.$store.dispatch("getComments", this.word);
        document.body.scrollTop = 0;
        document.documentElement.scrollTop = 0;
        this.textareaInputValue = null;
      }, 700);
    },
  },
};
</script>

<style scoped>
.word-data-container {
  display: flex;
  flex-direction: column;
  border-top: 1.5px solid #ffb000;
  max-width: 80vw;
  padding: 2%;
  margin-top: 2%;
  margin-left: auto;
  margin-right: auto;
}
h1 {
  font-weight: unset;
}
.outer{
  background-color: #22558A;
  padding:25px;
  border-radius: 10px;
}
.inner{
  background-color: #fff;
  color: #000000;
  font-family: 'Poppins', Arial, Verdana;
  border-radius: 20px;
  padding: 15px;
  word-wrap: break-word;
}
#word-definition{
  white-space: pre-wrap;
  font-size: 1.2em;
  font-weight: unset;
  color: #5e5e5e;
}
span {
  color: #ec4b43;
}
.author {
  color: #6eb1e1;
  font-weight: bold;
  text-align: left;
}
.author > img {
  width: 1.5em;
  height: 1.5em;
}
#header-comments {
  border-top: 1.5px solid #ffb000;
  padding: 2%;
}
.comment-word-form {
  display: flex;
  flex-direction: column;
  margin-top: 3%;
}
.comment-word-form > input[type="submit"] {
  background-color: #ffb000;
  color: white;
  border: none;
  border-radius: 30px;
  padding: 16px 42px;
  text-decoration: none;
  cursor: pointer;
  margin: 0 auto;
  margin-bottom: 2%;
}
textarea {
  outline: none;
  margin-top: 2%;
  margin-bottom: 2%;
  border-color: #1f1671;
  border-radius: 8px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
  box-shadow: none;
  resize: none;
  padding: 15px;
}
label {
  font-size: 1.3em;
}
@media only screen and (max-width: 800px) {
.word-data-container {
    font-size: 0.7em;
  }
.outer {
  padding: 10px;
   min-height: 30vh; 

}
.inner{
  min-height: 30vh;
}
}
</style>
