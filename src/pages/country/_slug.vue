<template>
  <div class="container">
    <div class="container_detail_country" v-if="arrayCountry.length > 0">
      <q-btn
        to="/"
        label="Back"
        icon="keyboard_backspace"
        :class="{
          'bg-custom-black': this.$store.state.myStore.darkMode,
          'bg-custom-white': this.$store.state.myStore.darkMode == false,
        }"
      />

      <div class="container_country">
        <img :src="arrayCountry[0].flags.png" alt="" />
        <div class="detail_country">
          <h1>{{ arrayCountry[0].name.common }}</h1>
          <div class="country_features">
            <div class="features">
              <p>
                <b>Official Name: </b>
                {{ arrayCountry[0].name.official }}
              </p>
              <p><b>Population: </b> {{ arrayCountry[0].population }}</p>
              <p><b>Region: </b> {{ arrayCountry[0].region }}</p>
              <p><b>Sub Region: </b> {{ arrayCountry[0].subregion }}</p>
              <p><b>Capital: </b> {{ arrayCountry[0].capital[0] }}</p>
            </div>
            <div class="features">
              <p><b>Top Level Domain: </b>{{ arrayCountry[0].tld[0] }}</p>
              <p>
                <b>Currencies: </b>
                <span
                  v-for="(item, name, index) in arrayCountry[0].currencies"
                  :key="index"
                  >{{ item.name }}</span
                >
              </p>
              <p>
                <b>Languajes: </b>
                <span
                  v-for="(item, name, index) in arrayCountry[0].languages"
                  :key="index"
                >
                  <span v-if="index < contarObj(arrayCountry[0].languages) - 1"
                    >{{ item }}, &nbsp;
                  </span>
                  <span v-else>{{ item }}</span>
                </span>
              </p>
            </div>
          </div>

          <div class="border_countries">
            <b>Border Countries: </b>
            <div style="max-width: 400px">
              <q-btn
                v-for="(item, name, index) in borders"
                :key="index"
                :label="item.name.common"
                :class="{
                  'bg-custom-black': this.$store.state.myStore.darkMode,
                  'bg-custom-white':
                    this.$store.state.myStore.darkMode == false,
                }"
                :to="{
                  name: 'country-slug',
                  params: { slug: item.name.common },
                }"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "boot/axios";
import { onMounted, ref, defineComponent } from "vue";
import { useRouter, useRoute } from "vue-router";
import { Loading } from "quasar";
export default defineComponent({
  name: "CountrySlug",

  data() {
    return {
      arrayCountry: [],
      borders: [],
    };
  },
  watch: {
    $route(to, from) {
      console.log("to", to, "from", from);
      if (to.params.slug != from.params.slug) {
        this.getDataInicial();
      }
    },
  },

  methods: {
    async getDataInicial() {
      Loading.show();
      await api
        .get("name/" + this.$route.params.slug + "?fullText=true")
        .then((response) => {
          this.arrayCountry = response.data;
          //   Loading.hide();
        });

      if (this.arrayCountry[0].borders) {
        var slugBorders = "";
        this.arrayCountry[0].borders.map(function (res, index) {
          slugBorders = slugBorders + res + ",";
        });
        // console.log("slugBorders", slugBorders);
        await api.get("alpha?codes=" + slugBorders).then((response) => {
          this.borders = response.data;
          Loading.hide();
        });
      } else {
        Loading.hide();
      }
    },
    contarObj(myobj) {
      let count = Object.keys(myobj).length;
      return count;
    },
  },

  mounted() {
    this.getDataInicial();
  },
});
</script>
