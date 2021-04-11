<template>
  <v-container bg>
    <v-card class="mx-auto my-12 elevation-1" max-width="400">
      <v-img :src="country.flag" height="200"></v-img>

      <v-card-title>{{ country.name }}</v-card-title>
      <v-card-subtitle> {{ country.capital }}</v-card-subtitle>

      <v-card-subtitle class="pb-0"> Região</v-card-subtitle>
      <v-card-text
        class="text--primary"
        @click="
          () =>
            this.$router.push({
              name: 'home-region',
              query: { region: country.region },
            })
        "
      >
        <a>{{ country.region }}</a>
      </v-card-text>
      <v-card-subtitle class="pb-0"> Sub-região</v-card-subtitle>
      <v-card-text class="text--primary">
        <div>{{ country.subregion }}</div>
      </v-card-text>
      <v-card-subtitle class="pb-0"> População</v-card-subtitle>
      <v-card-text class="text--primary">
        <div>{{ country.population }}</div>
      </v-card-text>
      <v-card-subtitle class="pb-0"> Idiomas</v-card-subtitle>
      <v-card-text class="text--primary">
        <template v-for="lang in country.languages">
          <div :key="lang.nativeName">{{ lang.nativeName }}</div>
        </template>
      </v-card-text>
      <div v-if="country.borders && country.borders.length > 0">
        <v-card-subtitle class="pb-0"> Vizinhos</v-card-subtitle>
        <template v-for="border in country.borders">
          <v-img
            @click.native="viewer(border.alpha3Code)"
            :key="border.alpha3Code"
            :src="border.flag"
            class="white--text align-end"
            gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
            height="200px"
          >
          </v-img>
        </template>
      </div>
      <div v-else>
        <v-card-subtitle class="pb-0"> Não possui vizinhos</v-card-subtitle>
      </div>
    </v-card>
    <v-overlay :value="isLoading">
      <v-progress-circular indeterminate size="80"></v-progress-circular>
    </v-overlay>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "country",
  data() {
    return {
      isLoading: false,
      country: {},
    };
  },
  created() {
    this.getInfo();
  },
  methods: {
    async getInfo() {
      this.isLoading = true;
      if (this.$route.params.id) {
        try {
          const response = await axios.get(`https://restcountries.eu/rest/v2/alpha/${this.$route.params.id}
`);
          this.country = response.data;
          let borders = [];
          for (let i in this.country.borders) {
            let border = this.country.borders[i];
            const response = await axios.get(`https://restcountries.eu/rest/v2/alpha/${border}
`);
            borders.push(response.data);
          }
          this.country.borders = borders;
        } catch (e) {
          console.log(e);
        } finally {
          this.isLoading = false;
        }
      } else {
        alert("URL INVÁLIDA");
      }
    },
    viewer(name) {
      this.$router.push({ name: "country-id", params: { id: name } });
    },
  },
};
</script>
