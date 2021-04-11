<template>
  <v-app-bar color="white" dark app clipped-left fixed>
    <v-img
      class="mx-2"
      :src="require('~/assets/logo.png')"
      max-height="50"
      max-width="50"
      contain
    ></v-img>
    <v-spacer />
    <v-btn
      class="ma-2"
      outlined
      color="indigo"
      v-if="this.$route.name == 'country-id'"
      @click="() => this.$router.push({ name: 'home-region' })"
    >
      <v-icon left dark> mdi-keyboard-backspace </v-icon>Voltar
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: "Header",
};
</script>
