<template>
  <q-page class="flex flex-center">
    <q-img
      src="https://scontent.fosl3-1.fna.fbcdn.net/v/t1.0-9/89266455_112961590318562_634611096208015360_n.jpg?_nc_cat=108&_nc_sid=85a577&_nc_ohc=HC3VlJYOkZEAX_dcpJY&_nc_ht=scontent.fosl3-1.fna&oh=6d32603581bb75296f13397538d814ed&oe=5EB72364"
      spinner-color="white"
      style="height: auto; max-width: 300px"
      img-class="my-custom-image"
      class="rounded-borders"
    >
      <q-btn
        to="/chat"
        label="Start a chat with Covidu"
        class="absolute-bottom text-subtitle1 text-center"
        color="primary"
      />
    </q-img>

    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="chat" to="/chat" color="primary" />
    </q-page-sticky>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>
