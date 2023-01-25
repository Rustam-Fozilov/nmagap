<template>
  <div class="tweet-container">
    <div class="tweet-post">
      <div class="tweet-avatar">
        <img :src="require('@/assets/emojis/' + tweet.avatar.image)" alt="avatar">
      </div>
      <div class="tweet-info">
        <div class="tweet-date">{{ tweet.date }}</div>
        <div class="tweet-text">{{ tweet.text }}</div>
        <div class="tweet-actions">
          <div @click="like" class="tweet-action">
            <div class="action-icon">
              <svg width="20" height="17" viewBox="0 0 20 17" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M5.95 1C3.21625 1 1 3.15216 1 5.80685C1 10.6137 6.85 14.9836 10 16C13.15 14.9836 19 10.6137 19 5.80685C19 3.15216 16.7837 1 14.05 1C12.376 1 10.8955 1.80712 10 3.04248C9.54356 2.41112 8.93717 1.89586 8.23219 1.54033C7.52721 1.1848 6.74438 0.999456 5.95 1Z" stroke="black" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>
            <div class="action-count">{{ tweet.likes }}</div>
          </div>
          <div @click="toggleComment()" class="tweet-action">
            <div class="action-icon">
              <svg width="18" height="17" viewBox="0 0 18 17" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M15.2222 1H2.77778C2.30628 1 1.8541 1.18592 1.5207 1.51687C1.1873 1.84782 1 2.29668 1 2.76471V16L4.08178 13.7059C4.3895 13.4768 4.76379 13.3529 5.14844 13.3529H15.2222C15.6937 13.3529 16.1459 13.167 16.4793 12.8361C16.8127 12.5051 17 12.0563 17 11.5882V2.76471C17 2.29668 16.8127 1.84782 16.4793 1.51687C16.1459 1.18592 15.6937 1 15.2222 1Z" stroke="black" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>
            <div v-if="tweet.comments" class="action-count">{{ tweet.comments.length }}</div>
            <div v-if="!tweet.comments" class="action-count">0</div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="isCommentOpened" class="tweet-comments">
      <div class="comment-field">
        <input @keyup.enter="sendComment" v-model="commentText" type="text" placeholder="Write a comment...">
        <svg @click="sendComment" width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M0.930446 0.0679051C0.822829 0.0140803 0.702266 -0.00846139 0.582471 0.00284431C0.462676 0.01415 0.348459 0.0588492 0.252812 0.131857C0.157165 0.204865 0.0839273 0.303251 0.0414301 0.415824C-0.001067 0.528396 -0.0111177 0.650636 0.0124215 0.768638L1.81633 7.00452C1.84996 7.12073 1.91571 7.22509 2.006 7.3056C2.0963 7.38612 2.20748 7.43952 2.32677 7.45967L9.64268 8.68499C9.98726 8.75314 9.98726 9.24686 9.64268 9.31501L2.32677 10.5403C2.20748 10.5605 2.0963 10.6139 2.006 10.6944C1.91571 10.7749 1.84996 10.8793 1.81633 10.9955L0.0124215 17.2314C-0.0111177 17.3494 -0.001067 17.4716 0.0414301 17.5842C0.0839273 17.6967 0.157165 17.7951 0.252812 17.8681C0.348459 17.9412 0.462676 17.9858 0.582471 17.9972C0.702266 18.0085 0.822829 17.9859 0.930446 17.9321L17.6452 9.57473C17.7518 9.52128 17.8415 9.43922 17.9041 9.33772C17.9668 9.23622 18 9.11928 18 9C18 8.88071 17.9668 8.76378 17.9041 8.66228C17.8415 8.56078 17.7518 8.47872 17.6452 8.42527L0.930446 0.0679051Z" fill="black"/>
        </svg>
      </div>
      <div v-if="tweet.comments" v-for="comment in tweet.comments" class="comments-info">
        <div class="tweet-avatar">
          <img :src="require('@/assets/emojis/' + comment.avatar)" alt="avatar">
        </div>
        <div class="comment-text">
          <div class="comment-date">{{ comment.date }}</div>
          {{ comment.comment }}
        </div>
      </div>
      <div v-if="!tweet.comments" class="comment-text">No comments yet, write first</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TheTweet",
  emits: ['load-tweet'],
  props: {
    tweet: {
      required: false,
      type: Object
    },
  },

  data() {
    return {
      isCommentOpened: false,
      commentText: '',
    }
  },

  methods: {
    like() {
      axios
          .patch('https://vue-mini-twitter-default-rtdb.firebaseio.com/tweets/' + this.tweet.id + '.json', {
            likes: this.tweet.likes + 1,
          })
          .then(res => {
            this.$emit('load-tweet')
          })
          .catch(e => {
            console.log(e)
          })
    },

    toggleComment() {
      this.isCommentOpened = !this.isCommentOpened
    },

    sendComment() {
      const date = new Date();
      const emojis = ["emoji-01.png", "emoji-02.png", "emoji-03.png", "emoji-04.png", "emoji-05.png", "emoji-06.png"]
      const randomImg = Math.floor(Math.random() * emojis.length);

      if(this.tweet.comments) {
        this.tweet.comments.push({
          date: date.toLocaleDateString() + ', ' + date.getHours() + ':' + date.getMinutes(),
          comment: this.commentText,
          avatar: emojis[randomImg]
        })
      } else {
        this.tweet.comments = [
          {
            date: date.toLocaleDateString() + ', ' + date.getHours() + ':' + date.getMinutes(),
            comment: this.commentText,
            avatar: emojis[randomImg]
          }
        ]
      }

      axios
          .patch('https://vue-mini-twitter-default-rtdb.firebaseio.com/tweets/' + this.tweet.id + '.json', {
            comments: this.tweet.comments,
          })
          .then(res => {
            this.commentText = ''
          })
          .catch(e => {
            console.log(e)
          })
    }
  }
}
</script>

<style scoped>
.commentOpen {
  display: none;
}

.tweet-container {
  background-color: white;
  padding: 20px;
  border-radius: 23px;
  margin-bottom: 30px;
}

.tweet-post {
  display: flex;
  align-items: center;
}

.tweet-avatar {
  width: 60px;
  height: 60px;
  background-color: v-bind(tweet.avatar.color);
  padding: 10px;
  border-radius: 10px;
  margin-right: 20px;
}

.tweet-avatar img {
  object-fit: contain;
  width: 60px;
  height: 60px;
}

.tweet-date {
  font-family: Montserrat-Regular;
  opacity: 0.5;
  font-size: 12px;
}

.tweet-text {
  font-family: Montserrat-Regular;
  font-size: 1rem;
  letter-spacing: -1px;
  margin: 10px 0;
}

.tweet-actions {
  display: flex;
}

.tweet-action {
  display: flex;
  margin-right: 20px;
}

.action-icon {
  margin-right: 5px;
  cursor: pointer;
}

.action-count {
  font-family: Montserrat-Regular;
  font-size: 1rem;
}

.tweet-comments {
  margin: 40px 0 40px 100px;
}

.comment-field {
  width: 80%;
  padding: 1rem;
  background-color: #f2f2f5;
  border-radius: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40px;
}

.comment-field svg {
  cursor: pointer;
}

.comment-field input {
  width: 90%;
  background: transparent;
  border: none;
  font-family: Montserrat-Regular;
  font-size: 1rem;
  outline: none;
}

.comments-info {
  display: flex;
  align-items: self-start;
  margin-bottom: 20px;
}

.comment-text {
  font-family: Montserrat-Regular;
  font-size: 1rem;
}

.comment-date {
  font-size: 12px;
  opacity: 0.5;
  margin-bottom: 10px;
}

@media only screen and (max-width: 500px) {
  .tweet-text {
    font-size: 16px;
  }

  .comment-field input {
    font-size: 12px;
  }

  .comment-text {
    font-size: 16px;
  }
}

</style>