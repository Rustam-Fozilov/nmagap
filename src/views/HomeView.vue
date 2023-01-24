<template>
  <div class="home-container">
    <the-tweet
        @load-tweet="loadTweets"
        v-for="tweet in allTweets" :key="tweet.id"
        :tweet="tweet"
        :comments="tweet.comments"
    ></the-tweet>

    <button @click="postModal = true" class="btn-make-tweet">MAKE TWEET</button>

    <div class="nav-link">
      <router-link to="/">HOME</router-link>
      <router-link to="/about">ABOUT</router-link>
      <p>Developer: Rustam Fozilov</p>
    </div>

    <div v-if="postModal" @click="postModal = false" class="modal-container"></div>
      <div v-if="postModal" class="post-modal">
        <div class="post-title">Post new tweet</div>
        <div class="post-text">
          <textarea v-model="tweetText" rows="5" placeholder="Type here..."></textarea>
        </div>
        <div class="post-send-btn">
          <button @click="sendTweet">TWEET</button>
        </div>
      </div>
    <div style="clear: both;"></div>
  </div>
</template>

<script>
import TheTweet from "@/components/TheTweet";
import axios from "axios";

export default {
  name: "HomeView",
  components: {TheTweet},

  data() {
    return {
      allTweets: [],
      postModal: false,
      tweetText: ''
    }
  },

  created() {
    this.loadTweets()
  },

  methods: {
    loadTweets() {
      try {
        axios
            .get('https://vue-mini-twitter-default-rtdb.firebaseio.com/tweets.json')
            .then((res) => {
              this.allTweets = Object.keys(res.data).map(key => {
                return {
                  id: key,
                  ...res.data[key]
                }
              })
            })

      } catch (e) {
        console.log(e);
      }
    },

    sendTweet() {
      const currentDate = new Date();

      const background = ["#F1D900", "#3CC3EE", "#50D93A", "#A864DD", "#FF3535", "#FF7B1C", "#3D3D3D", "#2F4FF8", "#C27A36", "#D9D9D9"]
      const emojis = ["emoji-01.png", "emoji-02.png", "emoji-03.png", "emoji-04.png", "emoji-05.png", "emoji-06.png", "emoji-07.png", "emoji-08.png", "emoji-09.png", "emoji-10.png"]
      const randomImg = Math.floor(Math.random() * emojis.length);
      const randomColor = Math.floor(Math.random() * background.length);

      try {
        axios
            .post('https://vue-mini-twitter-default-rtdb.firebaseio.com/tweets.json', {
              avatar: {
                image: emojis[randomImg],
                color: background[randomColor]
              },
              date: currentDate.toLocaleDateString(),
              likes: 0,
              text: this.tweetText,
              comments: []
            })
            .then((res) => {
              this.postModal = false;
              this.tweetText = ''
              this.loadTweets()
            })
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>

<style scoped>
.home-container {
  margin-top: 30px;
}

.btn-make-tweet {
  position: fixed;
  bottom: 100px;
  width: 720px;
  height: 70px;
  box-shadow: 0 10px 20px #96D9F9;
  border-radius: 20px;
  border: none;
  background-color: #00B2FF;
  color: white;
  font-family: Montserrat-SemiBold;
  font-size: 1rem;
  cursor: pointer;
}

.nav-link {
  position: relative;
  flex: auto 0;
  bottom: 0;
  text-align: center;
  margin-top: 120px;
}

.nav-link a {
  color: black;
  text-decoration: none;
  margin: 0 10px;
  font-family: Montserrat-Regular;
  font-size: 1rem;
}

.nav-link p {
  font-family: Montserrat-Regular;
  font-size: 16px;
  opacity: 0.5;
}

.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, .5);
}

.post-modal {
  border-radius: 30px;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 720px;
  padding: 40px;
  background-color: white;
}

.post-title {
  text-align: center;
  font-family: Montserrat-SemiBold;
  font-size: 24px;
}

.post-text {
  width: 95%;
}

.post-text textarea {
  width: 100%;
  margin: 20px 0;
  resize: none;
  padding: 16px;
  font-family: Montserrat-Regular;
  font-size: 1rem;
  border-radius: 18px;
  background-color: #f2f2f5;
  outline: none;
  border: none;
}

.post-send-btn button {
  width: 100%;
  height: 62px;
  border-radius: 14px;
  border: none;
  background-color: #00B2FF;
  font-family: Montserrat-SemiBold;
  font-size: 1rem;
  color: white;
  cursor: pointer;
  box-shadow: 0 10px 20px #96D9F9;
}

</style>