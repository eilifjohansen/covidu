<template>
  <q-item
    clickable
    class="grey-10"
    tag="a"
    target="_blank"
    rel="noreferrer"
    :href="link"
  >
    <q-item-section v-if="icon" avatar>
      <q-icon :name="icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label class="underline"
        >{{ title }}
        <svg
          aria-hidden="true"
          role="presentation"
          focusable="false"
          viewBox="0 0 24 24"
          class="q-icon notranslate"
        >
          <path
            d="M14,3V5H17.59L7.76,14.83L9.17,16.24L19,6.41V10H21V3M19,19H5V5H12V3H5C3.89,3 3,3.89 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V12H19V19Z"
          ></path>
        </svg>
      </q-item-label>
    </q-item-section>
  </q-item>
</template>

<script>
export default {
  name: "EssentialLink",

  props: {
    title: {
      type: String,
      required: true
    },
    caption: {
      type: String,
      default: ""
    },
    link: {
      type: String,
      default: "#"
    },
    icon: {
      type: String,
      default: ""
    }
  }
};
</script>
<style lang="scss" scoped>
.underline {
  text-decoration: underline !important;
  color: #1976d2;
}

.q-item__section--side > .q-icon {
  color: #1976d2;
}

:focus {
  outline: 1px solid black !important;
}
</style>
