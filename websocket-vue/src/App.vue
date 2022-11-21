<script>
export default {
  name: 'App',
  data() {
    return {
      message: "",
      socket: null,
      receivedMessage: "",
      showMsg: false
    }
  },
  mounted() {
    this.socket = new WebSocket("ws://localhost:9100/socket")
    this.socket.onmessage = (msg) => {
      this.acceptMessage(msg)
    }
  },
  methods: {
    sendMessage() {
      let msg = {
        "greeting": this.message
      }
      this.socket.send(JSON.stringify(msg))
    },
    acceptMessage(msg) {
      this.receivedMessage = msg.data
      this.showMsg = true
    }
  }
}
</script>

<template>
  <form :action="sendMessage" @click.prevent="onSubmit">
    <input v-model="message" type="text">
    <input type="submit" value="Send" @click="sendMessage">
  </form>
  <div v-if="showMsg">
    <h3>Message in a Websocket</h3>
    <p>
      {{receivedMessage}}
    </p>
    <button @click="showMsg = !showMsg">Dismiss</button>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
