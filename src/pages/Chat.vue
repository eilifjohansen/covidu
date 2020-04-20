<template>
  <q-page
    ref="pageChat"
    class="page-chat flex column"
    role="region"
    aria-label="chat window"
  >
    <div class="q-pa-md column col justify-end">
      <q-chat-message
        tabindex="0"
        :aria-live="message.ariaLive != 'off' ? 'polite' : message.ariaLive"
        v-for="message in messages"
        :key="message.id"
        :name="message.from"
        :text="[message.text]"
        :sent="message.from == 'Me' ? true : false"
        class="text-body2"
        :stamp="message.stamp"
        text-sanitize
        color="white"
      >
        <template v-slot:avatar v-if="message.from == 'Covidu' ? true : false">
          <img
            aria-hidden="true"
            role="presentation"
            alt=""
            class="q-message-avatar q-message-avatar--sent"
            src="statics/covidu.png"
          />
        </template>
      </q-chat-message>
    </div>
    <q-footer>
      <q-toolbar bordered bg-color="white">
        <q-form @submit="sendMessage" class="full-width">
          <q-input
            type="text"
            name="message"
            aria-label="message to send"
            v-model="newMessage"
            ref="newMessage"
            id="mymessage"
            bg-color="white"
            outlined
            rounded
            label="Message"
            dense
            class="text-body2"
          >
            <template v-slot:after>
              <q-btn
                size="25px"
                style="padding-left:5px;padding-right:5px;"
                aria-label="Send"
                @click="sendMessage"
                dense
                flat
                type="submit"
                class="sendFocus"
              >
                <q-icon name="send" size="1.2em" alt="Send" />
              </q-btn>
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

let uniqueId = uid();

export default {
  data() {
    return {
      newMessage: "",
      userId: uniqueId,
      ariaLive: "",
      messages: [
        {
          id: 1,
          text:
            "Hi, what country would you like to check the latest coronavirus stats in?",
          from: "Covidu",
          stamp: date.formatDate(Date.now(), "HH:mm"),
          ariaLive: "off"
        }
      ],
      online: true
    };
  },
  methods: {
    sendMessage() {
      this.$refs.newMessage.focus();

      if (this.newMessage.length >= 2 && this.newMessage.length <= 100) {
        this.messages.push({
          text: this.newMessage,
          from: "Me",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
      }

      if (this.newMessage.length == 1) {
        this.messages.push({
          text: this.newMessage,
          from: "Me",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
        this.messages.push({
          text:
            "Woups! I don't know any countries with only 1 character... Try again.",
          from: "Covidu",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
      }

      if (this.newMessage.length >= 101) {
        this.messages.push({
          text: "[large message]",
          from: "Me",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
        this.messages.push({
          text:
            "Woups! I can not respond to messages with more than 100 characters... Try again.",
          from: "Covidu",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
      }

      /* if (this.newMessage.length == 0) {
        this.messages.push({
          text: "[Empty message]",
          from: "Me",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
        this.messages.push({
          text: "Woups! That message looks empty... Try saying a country name.",
          from: "Covidu",
          stamp: date.formatDate(Date.now(), "HH:mm")
        });
      }*/

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
                    text: result,
                    from: "Covidu",
                    stamp: date.formatDate(Date.now(), "HH:mm")
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
                    stamp: date.formatDate(Date.now(), "HH:mm")
                  });
                }
              }

              if (result == "Accepted") {
                this.messages.push({
                  text:
                    "Oh no! The place i get stats from is down for maintenance. Please try again later.",
                  from: "Covidu",
                  stamp: date.formatDate(Date.now(), "HH:mm")
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
                stamp: date.formatDate(Date.now(), "HH:mm")
              });
            }
          });

        if (navigator.onLine == false) {
          // true|false
          this.messages.push({
            text:
              "You have lost connection to the internet... Try to reload the page.",
            from: "Covidu",
            stamp: date.formatDate(Date.now(), "HH:mm")
          });
        }
      }

      this.clearMessage();
    },
    clearMessage() {
      this.newMessage = "";
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
button:focus {
  outline: 1px solid black !important;
}

input:focus {
  box-shadow: 0 0 0pt 0pt $primary;
}

.q-footer .q-icon {
  color: #1976d2;
}

.q-layout__section--marginal {
  background-color: #fff;
  color: #fff;
  border-top: 1px solid rgba(0, 0, 0, 0.12);
}

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

.q-message-text-content--received .q-message-stamp {
  text-align: right;
}

.q-message-text--received {
  background: #f1f0f0;
  color: #f1f0f0;
}

.q-message-text-content--received {
  color: #000;
  white-space: pre-line;
}

.q-message-avatar--sent {
  margin-left: 0px;
}

.q-field--outlined .q-field__control:before {
  border: 1px solid #1976d2;
  transition: border-color 0.36s cubic-bezier(0.4, 0, 0.2, 1);
  background: #f1f0f0;
}

.q-field__label {
  color: #444;
}

.q-field--dense .q-field__control,
.q-field--dense .q-field__marginal {
  height: 50px;
}

.q-field--dense .q-field__label {
  top: 15px;
}

.q-toolbar {
  min-height: 60px;
}
</style>
