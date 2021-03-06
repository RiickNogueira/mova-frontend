<template>
  <v-container bg>
    <v-row>
      <v-col class="d-flex" cols="12" sm="4">
        <v-select
          label="Filtrar por"
          v-model="filterOption"
          :items="filterOptions"
          item-text="name"
          return-object
          dense
          outlined
          rounded
          @change="getOptions"
        ></v-select>
      </v-col>
      <v-col class="d-flex" cols="12" sm="4"
        ><v-autocomplete
          :label="`${
            filterOption ? filterOption.name : 'Selecione o tipo do filtro'
          }`"
          v-model="filter"
          :items="options"
          :item-text="`${filterOption ? filterOption.field : ''}`"
          dense
          outlined
          rounded
        ></v-autocomplete
      ></v-col>
      <v-col class="d-flex" cols="12" sm="4"
        ><v-btn depressed rounded color="primary" @click="getCountries">
          PESQUISAR
        </v-btn></v-col
      ></v-row
    >
    <div v-if="countries.length > 1">
      <v-data-iterator
        :items="countries"
        hide-default-footer
        fluid
        class="elevation-1"
        :page.sync="page"
        :items-per-page="itemsPerPage"
        @page-count="pageCount = $event"
      >
        <template v-slot:default="props">
          <v-row>
            <v-col
              v-for="(item, index) in props.items"
              :key="index"
              cols="12"
              md="4"
            >
              <v-card @click.native="viewer(item)">
                <v-img
                  gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.3)"
                  height="200"
                  :src="item.flag"
                ></v-img>
              </v-card>
            </v-col>
          </v-row>
        </template>
      </v-data-iterator>
      <div class="text-center pt-2">
        <v-pagination v-model="page" :length="pageCount"></v-pagination>
      </div>
    </div>
    <div v-else-if="countries.length == 1" class="pa-2 d-flex justify-center">
      <v-col cols="12" md="4">
        <v-card @click.native="viewer(countries[0])">
          <v-img
            gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.3)"
            height="200"
            :src="countries[0].flag"
          ></v-img> </v-card
      ></v-col>
    </div>
    <div v-else>Nenhum resultado</div>
    <v-overlay :value="isLoading">
      <v-progress-circular indeterminate size="80"></v-progress-circular>
    </v-overlay>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Index",
  data() {
    return {
      isLoading: false,
      page: 1,
      pageCount: 0,
      itemsPerPage: 12,
      options: [],
      countries: [],
      filter: "",
      filterOption: {
        name: "País",
        api: "https://restcountries.eu/rest/v2/name/",
        field: "name",
      },
      filterOptions: [
        { name: "Selecione uma opção", api: null, field: null },
        {
          name: "Região",
          api: "https://restcountries.eu/rest/v2/region/",
          field: "region",
        },
        {
          name: "Capital",
          api: "https://restcountries.eu/rest/v2/capital/",
          field: "capital",
        },
        {
          name: "País",
          api: "https://restcountries.eu/rest/v2/name/",
          field: "name",
        },
        {
          name: "Código de ligação",
          api: "https://restcountries.eu/rest/v2/callingcode/",
          field: "callingCodes",
        },
      ],
    };
  },
  created() {
    const region = this.$route.query.region;
    const regions = [
      "Africa",
      "Americas",
      "Asia",
      "Europe",
      "Oceania",
      "Polar",
    ];
    if (regions.includes(region)) {
      this.filterOption = this.filterOptions[1];
      this.filter = region;
      this.getCountries();
    }
    this.getOptions();
  },
  methods: {
    async getOptions() {
      try {
        this.options = [];
        this.isLoading = true;
        const response = await axios.get(`https://restcountries.eu/rest/v2/all?fields=${this.filterOption.field}

`);
        this.options = response.data;
      } catch (e) {
        console.log(e);
      } finally {
        this.isLoading = false;
      }
    },
    async getCountries() {
      if (this.$route.query.region) {
        this.$router.replace({ query: null });
      }
      if (!this.filter || !this.filterOption || this.filterOption == "") {
        alert("Filtro inválido");
      } else {
        try {
          this.countries = [];
          this.isLoading = true;
          const response = await axios.get(
            `${this.filterOption.api}${this.filter}`
          );
          this.countries = response.data;
        } catch (e) {
          console.log(e);
        } finally {
          this.isLoading = false;
        }
      }
    },
    viewer(item) {
      this.$router.push({
        name: "country-id",
        params: { id: item.alpha3Code },
      });
    },
  },
};
</script>
