<template>
  <div class="container">
    <div class="rowFilterHome">
      <q-input
        v-model="text"
        input-class="text-left"
        label="Search for a country"
        class="searchCountryInput"
      >
        <template v-slot:prepend>
          <q-icon v-if="text === ''" name="search" />
          <q-icon
            v-else
            name="clear"
            class="cursor-pointer"
            @click="text = ''"
          />
        </template>
      </q-input>

      <q-select
        v-model="filterRegion"
        :options="optionsRegions"
        label="Filter by Region"
        class="filterCountrySelect"
      />
    </div>

    <div class="container-countrys">
      <div class="row">
        <card-country-loop
          v-for="(item, index) in arrayCountries"
          :key="index"
          :arrayCountry="item"
          :cant="arrayCountries.length"
        >
        </card-country-loop>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { Loading, Notify } from "quasar";
import cardCountryLoop from "../components/home/cardCountryLoop.vue";
import { api } from "boot/axios";
export default defineComponent({
  components: { cardCountryLoop },
  name: "IndexPage",
  watch: {
    filterRegion(value) {
      if (value != null) {
        //  console.log(value);
        if (value == "All") {
          this.getCountrys("all");
        } else {
          this.getCountrys("region/" + value);
        }
      }
    },
    text(value) {
      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.filterRegion = null;
        if (value == "") {
          this.getCountrys("all");
        } else {
          this.getCountrys("name/" + value);
        }
      }, 500);
    },
  },
  data() {
    return {
      text: "",
      filterRegion: null,
      optionsRegions: ["Africa", "America", "Asia", "Europe", "Oceania", "All"],
      arrayCountries: [],
      timeout: null,
    };
  },
  methods: {
    async getCountrys(param) {
      Loading.show();
      await api
        .get(param)
        .then((response) => {
          console.log(response);
          this.arrayCountries = response.data;
          Loading.hide();
        })
        .catch((response) => {
          Loading.hide();
          this.$q.notify({
            type: "info",
            message: "No hay resultados para esta busqueda",
          });
        });
    },
  },
  mounted() {
    this.getCountrys("all");
  },
});
</script>
