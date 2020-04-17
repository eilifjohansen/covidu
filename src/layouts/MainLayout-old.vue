<template>
  <q-layout view="lHh LpR lFr">
    <q-header class="bg-primary text-white collapse">
      <q-toolbar>
        <q-btn
          class="only-mobile"
          dense
          flat
          round
          icon="menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title class="only-mobile">
          <q-avatar>
            <img src="statics/covidu.png" />
          </q-avatar>
          Covidu
        </q-toolbar-title>

        <q-toolbar-title class="only-desktop">
          <!--<div class="absolute-center">-->
          Chat with Covidu
          <!-- </div>-->
        </q-toolbar-title>

        <q-btn dense flat round icon="info" @click="right = !right" />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above>
      <q-toolbar class="bg-primary text-white collapse">
        <!-- <q-btn
          dense
          flat
          round
          icon="menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />-->

        <q-toolbar-title>
          <!--<div class="absolute-center">-->
          <q-avatar>
            <img src="statics/covidu.png" />
          </q-avatar>
          Covidu
          <!-- </div>-->
        </q-toolbar-title>
      </q-toolbar>

      <q-list>
        <HomeLink v-for="link in homeLinks" :key="link.title" v-bind="link" />

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-drawer show-if-above v-model="right" side="right">
      <div
        class="text-center bg-transparent"
        style="margin-top: 10px; padding-bottom: 5px;  border-bottom: 1px solid rgba(0, 0, 0, 0.12);"
      >
        <q-avatar size="106px" class="q-mb-sm">
          <img src="statics/covidu.png" />
        </q-avatar>
        <div class="text-center">
          <div class="text-weight-bold, text-h6" style="margin-top:0px">
            Covidu
          </div>
        </div>
      </div>
      <div style="padding: 10px">
        <p>Hi, I'm Covidu, the Coronavirus Updates Chatbot.</p>
        <p>
          Name a country, and I will get you the latest Coronavirus statistics
          that I can get hold of.
        </p>

        <p>
          My data source is:
          <a href="https://coronavirus.jhu.edu/" target="_blank"
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
      <!-- drawer content -->
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
import HomeLink from "components/HomeLink";

export default {
  name: "MainLayout",

  components: {
    EssentialLink,
    HomeLink
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
          icon: "link",
          link: "https://m.me/covidu"
        },
        {
          title: "Covidu on Telegram",
          icon: "link",
          link: "https://t.me/covidu_bot"
        },
        {
          title: "About the project",
          icon: "link",
          link: "https://devpost.com/software/covidu"
        },
        {
          title: "Like us on Facebook",
          icon: "link",
          link: "https://www.facebook.com/covidu"
        }
      ]
    };
  }
};
</script>
<style lang="stylus">
.q-page {
  border-left: 1px solid rgba(0, 0, 0, 0.12);
  border-right: 1px solid rgba(0, 0, 0, 0.12);
}

@media screen and (min-width: 1009px) {
  .only-mobile {
    display: none;
  }
}

@media screen and (max-width: 1008px) {
  .only-desktop {
    display: none;
  }
}
</style>
