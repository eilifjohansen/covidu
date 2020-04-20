<template>
  <keep-alive id="q-app">
    <router-view />
  </keep-alive>
</template>

<script>
import "whatwg-fetch";
export default {
  name: "App"
};
</script>

<style lang="scss">
.notifyme {
  .q-notification__actions {
    color: black;
  }

  .q-btn__wrapper {
    background: white;
    text-transform: initial;
  }
}
</style>
