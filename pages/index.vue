<template>
  <v-container bg>
    <v-row align="center">
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
          :label="`${filterOption ? filterOption.name : ''}`"
          v-model="filter"
          :items="options"
          :item-text="`${filterOption ? filterOption.field : ''}`"
          dense
          outlined
          rounded
        ></v-autocomplete
      ></v-col>
      <v-col class="d-flex" cols="12" sm="4"
        ><v-btn depressed rounded color="primary" @click="getCountrys">
          PESQUISAR
        </v-btn></v-col
      ></v-row
    >

    <v-data-iterator
      :items="countrys"
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
            <v-card>
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
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Index",
  data() {
    return {
      page: 1,
      pageCount: 0,
      itemsPerPage: 12,
      options: [],
      countrys: [],
      filter: null,
      filterOption: null,
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
    //this.getOptions();
  },
  methods: {
    async getOptions() {
      try {
        const response = await axios.get(`https://restcountries.eu/rest/v2/all?fields=${this.filterOption.field}

`);
        this.options = response.data;
      } catch (e) {
        console.log(e);
      }
    },
    async getCountrys() {
      try {
        const response = await axios.get(
          `${this.filterOption.api}${this.filter}`
        );
        this.countrys = response.data;
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>
