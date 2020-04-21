<template>
  <q-layout view="hHh lpR fFr">
    <q-header class="bg-primary text-white collapse">
      <q-toolbar>
        <q-toolbar-title>
          <!-- <q-btn @click="doUpdate()" class="homeButton" to="/" role="link">
            <q-avatar size="42px">
              <img src="statics/covidu.png" alt="The coronavirus chatbot" />
            </q-avatar>
            <h1 class="text-h5" style="padding-left: 10px; margin: 0px">
              Covidu
            </h1>
          </q-btn>-->

          <div class="items-center row">
            <h1
              class="q-toolbar__title ellipsis col-shrink"
              style="line-height: 0; margin-top: 5px; margin-bottom: 5px"
            >
              <q-avatar
                size="48px"
                tabindex="0"
                to="/"
                role="link"
                @click="doUpdate()"
              >
                <img src="statics/covidu.png" alt="The coronavirus chatbot" />
              </q-avatar>

              Covidu
            </h1>
          </div>
        </q-toolbar-title>

        <q-btn
          size="25px"
          class="only-mobile"
          dense
          flat
          @click="right = !right"
          aria-label="Info"
          style="padding-left:5px;padding-right:5px;"
        >
          <q-icon name="info" size="1.2em" alt="Info" />
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer
      show-if-above
      v-model="right"
      side="right"
      bordered
      no-swipe-close
      no-swipe-open
    >
      <div
        class="text-center bg-transparent"
        style="margin-top: 10px; padding-bottom: 5px;  border-bottom: 1px solid rgba(0, 0, 0, 0.12);"
      >
        <q-avatar size="106px" class="q-mb-sm">
          <img src="statics/covidu.png" alt="Profile picture of Covidu" />
        </q-avatar>
        <div class="text-center">
          <h2
            class="text-weight-bold, text-h6"
            style="margin-top:0px;margin-bottom: 0px;"
          >
            Covidu – The Coronavirus Updates Chatbot
          </h2>
        </div>
      </div>
      <div
        style="padding: 10px; border-bottom: 1px solid rgba(0, 0, 0, 0.12);"
        class="text-body2"
      >
        <p style="margin-top: 16px">
          Hi, I'm Covidu!
        </p>
        <p>
          Name a country, and I will get you the latest Coronavirus statistics
          that I can get hold of.
        </p>

        <p>
          Data source:
          <a
            href="https://coronavirus.jhu.edu/"
            target="_blank"
            style="color:#1976d2"
            rel="noreferrer"
            >Johns Hopkins CSSE
            <svg
              aria-hidden="true"
              role="presentation"
              focusable="false"
              viewBox="0 0 24 24"
              class="q-icon notranslate"
            >
              <path
                d="M14,3V5H17.59L7.76,14.83L9.17,16.24L19,6.41V10H21V3M19,19H5V5H12V3H5C3.89,3 3,3.89 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V12H19V19Z"
              ></path></svg></a
          >.
        </p>
      </div>
      <q-list style="margin-top: 10px">
        <EssentialLink
          class="text-body2"
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink";

export default {
  name: "MainLayout",

  components: {
    EssentialLink
  },

  data() {
    return {
      right: false,
      leftDrawerOpen: false,
      homeLinks: [
        {
          title: "Chat with Covidu",
          caption: "Chat with Covidu",
          icon: "chat",
          link: "/"
        }
      ],
      essentialLinks: [
        {
          title: "Covidu on Messenger",
          icon: "chat",
          link: "https://m.me/covidu"
        },
        {
          title: "About the project",
          icon: "info",
          link: "https://devpost.com/software/covidu"
        },
        {
          title: "Like us on Facebook",
          icon: "share",
          link: "https://www.facebook.com/covidu"
        }
      ]
    };
  },
  methods: {
    doUpdate() {
      window.location.reload(true);
    }
  }
};
</script>
<style>
.q-btn__wrapper:before {
  box-shadow: none;
}

.q-btn__wrapper {
  padding-left: 0px;
}
</style>
<style scoped>
:focus {
  box-shadow: 0 0 0pt 1pt #fff;
}

.homeButton .q-btn__wrapper:before {
  box-shadow: none;
}

.homeButton .q-btn__wrapper {
  padding-left: 0px;
  text-transform: initial;
}

@media screen and (min-width: 1006px) {
  .only-mobile {
    display: none !important;
  }
}

.body.desktop .q-focusable:focus > .q-focus-helper,
body.desktop .q-manual-focusable--focused > .q-focus-helper,
body.desktop .q-hoverable:hover > .q-focus-helper {
  /* background: currentColor; */
  opacity: 0;
}
</style>
