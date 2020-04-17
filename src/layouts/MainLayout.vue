<template>
  <q-layout view="hHh lpR fFr">
    <q-header class="bg-primary text-white collapse">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="statics/covidu.png" alt="The coronavirus chatbot" />
          </q-avatar>
          Covidu
        </q-toolbar-title>

        <q-btn
          dense
          flat
          round
          icon="info"
          @click="right = !right"
          aria-label="Info"
        />
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="right" side="right" bordered>
      <div
        class="text-center bg-transparent"
        style="margin-top: 10px; padding-bottom: 5px;  border-bottom: 1px solid rgba(0, 0, 0, 0.12);"
      >
        <q-avatar size="106px" class="q-mb-sm">
          <img src="statics/covidu.png" />
        </q-avatar>
        <div class="text-center">
          <div class="text-weight-bold, text-h6" style="margin-top:0px">
            Covidu – The Coronavirus Updates Chatbot
          </div>
        </div>
      </div>
      <div style="padding: 10px; border-bottom: 1px solid rgba(0, 0, 0, 0.12);">
        <p style="margin-top: 16px">
          Hi, I'm Covidu!
        </p>
        <p>
          Name a country, and I will get you the latest Coronavirus statistics
          that I can get hold of.
        </p>

        <p>
          My data source is:
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
  }
};
</script>
