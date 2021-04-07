<template>
  <v-app-bar color="white" dark app clipped-left fixed>
    <v-img
      class="mx-2"
      :src="require('~/assets/logo.png')"
      max-height="50"
      max-width="50"
      contain
    ></v-img>
  </v-app-bar>
</template>

<script>
export default {
  name: "Header",
};
</script>
