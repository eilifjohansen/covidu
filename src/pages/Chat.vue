<template>
  <q-page ref="pageChat" class="page-chat flex column">
    <div class="q-pa-md column col justify-end">
      <q-chat-message
        v-for="message in messages"
        :key="message.id"
        :name="message.from"
        :text="[message.text]"
        :sent="message.from == 'Me' ? true : false"
        class="text-body2"
      >
        <template v-slot:avatar v-if="message.from == 'Covidu' ? true : false">
          <img
            class="q-message-avatar q-message-avatar--sent"
            src="statics/covidu.png"
          />
        </template>
      </q-chat-message>
    </div>
    <q-footer>
      <q-toolbar>
        <q-form @submit="sendMessage" class="full-width">
          <q-input
            v-model="newMessage"
            ref="newMessage"
            id="mymessage"
            bg-color="white"
            outlined
            rounded
            label="Message"
            dense
          >
            <template v-slot:after>
              <q-btn
                @click="sendMessage"
                round
                dense
                flat
                color="white"
                icon="send"
              />
            </template>
          </q-input>
        </q-form>
      </q-toolbar>
    </q-footer>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newMessage: "",
      messages: [
        {
          id: 1,
          text:
            "Hi, what country would you like to check the latest coronavirus stats in?",
          from: "Covidu"
        }
      ]
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.length >= 4) {
        this.messages.push({
          text: this.newMessage,
          from: "Me"
        });
      }

      if (this.newMessage.length >= 1 && this.newMessage.length <= 3) {
        this.messages.push({
          text: this.newMessage,
          from: "Me"
        });
        this.messages.push({
          text:
            "Woups! I don't know any countries with less than 4 characters... Try again.",
          from: "Covidu"
        });
      }

      if (this.newMessage.length == 0) {
        this.messages.push({
          text: "[Empty message]",
          from: "Me"
        });
        this.messages.push({
          text: "Woups! That message looks empty... Try saying a country name.",
          from: "Covidu"
        });
      }

      const payload = {
        location: this.newMessage,
        webhook: "https://webhook.site/0878e958-df57-4a07-8f59-b43a4d888609",
        userid: "123"
      };

      if (this.newMessage.length >= 4) {
        fetch("https://hook.integromat.com/dtfbvlarirwglzhfsav0inoshn9kggxs", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(payload)
        }).then(response => {
          response.text().then(result => {
            this.messages.push({
              text: result.replace(/\n/g, "<br />"),
              from: "Covidu"
            });
          });
        });
      }

      this.clearMessage();
    },
    clearMessage() {
      this.newMessage = "";
      // this.$refs.newMessage.focus()
    },
    scrollToBottom() {
      let pageChat = this.$refs.pageChat.$el;
      setTimeout(() => {
        window.scrollTo(0, pageChat.scrollHeight);
      }, 20);
    }
  },
  watch: {
    messages: function(val) {
      if (Object.keys(val).length) {
        this.scrollToBottom();
        setTimeout(() => {
          this.showMessages = true;
        }, 200);
      }
    }
  }
};
</script>
<style lang="scss">
.q-message-text--sent {
  background: #1976d2;
  color: #1976d2;
}

.q-message-text-content--sent {
  color: #fff;
}

.q-message-text--received {
  background: #f1f0f0;
  color: #f1f0f0;
}

.q-message-text-content--received {
  color: #000;
}
</style>
