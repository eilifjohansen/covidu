<template>
  <div id="q-app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style lang="scss">
.notifyme {
  .q-notification__actions {
    color: primary;
  }

  .q-btn__wrapper {
    display: none;
    background: white;
  }
}
</style>
