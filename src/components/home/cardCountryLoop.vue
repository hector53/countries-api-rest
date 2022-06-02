<template>
  <div
    class="col-md-3 col-sm-6 col-xs-12"
    :class="{ columCard: cant > 3, columCardFull: cant < 3 }"
    v-if="arrayCountry.capital"
  >
    <router-link
      :to="{
        name: 'country-slug',
        params: { slug: arrayCountry.name.common },
      }"
    >
      <q-card class="cardCountryLoop">
        <img :src="arrayCountry.flags.png" />

        <q-card-section>
          <div class="text-h6">
            <b>{{ arrayCountry.name.common }}</b>
          </div>
          <div class="text-subtitle2">
            <b>Population: </b><span>{{ arrayCountry.population }}</span>
          </div>
          <div class="text-subtitle2">
            <b>Region: </b><span>{{ arrayCountry.region }}</span>
          </div>
          <div class="text-subtitle2">
            <b>Capital: </b><span>{{ arrayCountry.capital[0] }}</span>
          </div>
        </q-card-section>
      </q-card>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "cardCountryLoop",
  props: ["arrayCountry", "cant"],
};
</script>
