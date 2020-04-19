<template>
  <q-page ref="pageChat" class="page-chat flex column">
    <div class="q-pa-md column col justify-end">
      <q-chat-message
        v-for="message in messages"
        :key="message.id"
        :name="message.from"
        :text="[message.text]"
        :text-sanitize="message.from == 'Me' ? true : false"
        :sent="message.from == 'Me' ? true : false"
        class="text-body2"
        :stamp="message.stamp"
      >
        <template v-slot:avatar v-if="message.from == 'Covidu' ? true : false">
          <img
            aria-hidden="true"
            role="presentation"
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
                aria-label="Send"
                @click="sendMessage"
                dense
                flat
                color="white"
                icon="send"
                type="submit"
              />
            </template>
          </q-input>
        </q-form>
      </q-toolbar>
    </q-footer>

    <!-- place QPageSticky at end of page -->
    <q-page-sticky expand position="top" v-if="online == false">
      <q-toolbar class="bg-negative text-white">
        <br />
        You have lost connection to the internet. <br />Try to reload the
        page.<br /><br />
      </q-toolbar>
    </q-page-sticky>
  </q-page>
</template>

<script>
import { date, uid } from "quasar";

let timeStamp = Date.now();
let timeNow = date.formatDate(timeStamp, "HH:mm");
let uniqueId = uid();

export default {
  data() {
    return {
      newMessage: "",
      userId: uniqueId,
      messages: [
        {
          id: 1,
          text:
            "Hi, what country would you like to check the latest coronavirus stats in?",
          from: "Covidu",
          stamp: timeNow
        }
      ],
      online: true
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.length >= 2 && this.newMessage.length <= 100) {
        this.messages.push({
          text: this.newMessage,
          from: "Me",
          stamp: timeNow
        });
      }

      if (this.newMessage.length == 1) {
        this.messages.push({
          text: this.newMessage,
          from: "Me",
          stamp: timeNow
        });
        this.messages.push({
          text:
            "Woups! I don't know any countries with only 1 character... Try again.",
          from: "Covidu",
          stamp: timeNow
        });
      }

      if (this.newMessage.length >= 101) {
        this.messages.push({
          text: "[large message]",
          from: "Me",
          stamp: timeNow
        });
        this.messages.push({
          text:
            "Woups! I can not respond to messages with more than 100 characters... Try again.",
          from: "Covidu",
          stamp: timeNow
        });
      }

      if (this.newMessage.length == 0) {
        this.messages.push({
          text: "[Empty message]",
          from: "Me",
          stamp: timeNow
        });
        this.messages.push({
          text: "Woups! That message looks empty... Try saying a country name.",
          from: "Covidu",
          stamp: timeNow
        });
      }

      const payload = {
        message: this.newMessage,
        userid: this.userId,
        timestamp: Date.now()
      };

      if (this.newMessage.length >= 2 && this.newMessage.length <= 100) {
        fetch("https://hook.integromat.com/dtfbvlarirwglzhfsav0inoshn9kggxs", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(payload)
        })
          .then(response => {
            response.text().then(result => {
              if (result != "Accepted") {
                if (result != "") {
                  this.messages.push({
                    text: result.replace(/\n/g, "<br />"),
                    from: "Covidu",
                    stamp: timeNow
                  });
                }
              }

              if (result != "Accepted") {
                if (result == "") {
                  this.messages.push({
                    text:
                      "Oh no! There seems to be an error. I could not find any response message (status code: " +
                      response.status +
                      ").",
                    from: "Covidu",
                    stamp: timeNow
                  });
                }
              }

              if (result == "Accepted") {
                this.messages.push({
                  text:
                    "Oh no! The place i get stats from is down for maintenance. Please try again later.",
                  from: "Covidu",
                  stamp: new Date()
                    .toLocaleTimeString()
                    .replace(/(:\d{2}| [AP]M)$/, "")
                });
              }
            });
          })
          .catch(err => {
            console.log(err);
            if (navigator.onLine != false) {
              this.messages.push({
                text:
                  "Woups! Something went wrong. We're working on fixing it.",
                from: "Covidu",
                stamp: new Date()
                  .toLocaleTimeString()
                  .replace(/(:\d{2}| [AP]M)$/, "")
              });
            }
          });

        if (navigator.onLine == false) {
          // true|false
          this.messages.push({
            text:
              "You have lost connection to the internet... Try to reload the page.",
            from: "Covidu",
            stamp: new Date()
              .toLocaleTimeString()
              .replace(/(:\d{2}| [AP]M)$/, "")
          });
        }
      }

      this.clearMessage();
    },
    clearMessage() {
      this.newMessage = "";
      // this.$refs.newMessage.focus()
    },
    onLine() {
      if (navigator.onLine != false) {
        this.online = false;
      }
    },
    scrollToBottom() {
      let pageChat = this.$refs.pageChat.$el;
      setTimeout(() => {
        window.scrollTo(0, pageChat.scrollHeight);
      }, 20);
    }
  },
  updated() {
    if (navigator.onLine == false) {
      this.online = false;
    }

    if (navigator.onLine != false) {
      this.online = true;
    }
  },
  mounted() {
    if (navigator.onLine == false) {
      this.online = false;
    }

    if (navigator.onLine != false) {
      this.online = true;
    }

    if ("serviceWorker" in navigator) {
      navigator.serviceWorker.getRegistrations().then(function(registrations) {
        for (let registration of registrations) {
          registration.update();
        }
      });
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
  text-align: right;
}

.q-message-text-content--sent .q-message-stamp {
  color: #fff !important;
}

.q-message-text--received {
  background: #f1f0f0;
  color: #f1f0f0;
}

.q-message-text-content--received {
  color: #000;
}

.q-message-avatar--sent {
  margin-left: 0px;
}
</style>
