<template>
  <h1 style="text-align: center">Realtime chat app</h1>
  <div class="wrapper">
    <div class="chat">
      <div class="chat-messages">
        <div class="messages" ref="autoScroll">
          <div v-for="message in allMessages" :key="message.id" class="message">
            <p>{{ message.text }}</p>
            <div><span>{{ message.username }}</span></div>
          </div>
        </div>
        <form>
          <textarea
              @keyup.enter="sendMessage"
              v-model="message"
              placeholder="Type message..."
              class="form-control"
              rows="3"
          ></textarea>
<!--          <button @click="sendMessage" class="btn-primary">-->
<!--            SEND-->
<!--          </button>-->
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',

  data() {
    return {
      allMessages: [],
      message: '',
      username: '',
    }
  },

  beforeMount() {
    this.username = prompt('Ismingizni kiriting')
    this.getAllMessages();
  },

  updated() {
    this.$refs['autoScroll'].scrollTop = 9999999999;
  },

  methods: {
    getAllMessages() {
      try {
        axios
            .get('https://vue-realtime-chat-a5aec-default-rtdb.firebaseio.com/messages.json')
            .then((res) => {
              this.allMessages = Object.keys(res.data).map(key => {
                return {
                  id: key,
                  ...res.data[key]
                }
              })
              console.log(this.allMessages)
            })

      } catch (e) {
        console.log(e)
      }
    },

    sendMessage() {
      try {
        axios
            .post('https://vue-realtime-chat-a5aec-default-rtdb.firebaseio.com/messages.json', {
              text: this.message,
              username: this.username
            })
            .then((res) => {
              this.message = ''
              // this.getAllMessages();
            })
      } catch (e) {
        console.log(e)
      }
    }
  },

  watch: {
    allMessages() {
      this.getAllMessages();
    }
  }
}
</script>

<style>
html,
body {
  font-family: Arial, Helvetica, sans-serif;
  background-color: #f2f2f5;
}

.my-message {
  background-color: #C8CEDE !important;
}

.wrapper {
  max-width: 40%;
  min-width: 720px;
  margin: 70px auto;
}

.chat {
  background-color: white;
  border-radius: 20px;
  padding: 20px;
  height: 600px;
}

.chat-messages {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  flex: 1;
  padding: 30px;
}

.messages {
  flex: 1;
  overflow: auto;
}

.message {
  margin-bottom: 20px;
}

.message p {
  display: inline-flex;
  border-radius: 8px;
  background-color: #f2f2f5;
  padding: 12px 20px;
  margin: 0;
}

.message span {
  opacity: 0.5;
  font-size: 14px;
}

.chat-messages form {
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid rgb(200, 200, 200);
}

.chat-messages form textarea {
  width: 100%;
  resize: none;
  outline: none;
  border: none;
  background-color: #f5f5f5;
  border-radius: 10px;
  padding: 10px;
  height: 80px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  margin-bottom: 30px;
}

.btn-primary {
  padding: 12px 35px;
  margin-top: 10px;
  border: none;
  font-size: 16px;
  border-radius: 10px;
  background: #151515;
  color: white;
  cursor: pointer;
}
</style>
