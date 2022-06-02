<template>
  <q-layout view="lHh Lpr lFf">
    <q-header
      elevated
      class="container"
      :class="{
        'bg-custom-black': this.$store.state.myStore.darkMode,
        'bg-custom-white': this.$store.state.myStore.darkMode == false,
      }"
    >
      <q-toolbar
        class="toolbarCountries"
        :class="{
          'bg-custom-black': this.$store.state.myStore.darkMode,
          'bg-custom-white': this.$store.state.myStore.darkMode == false,
        }"
        style="max-width: 1200px; padding-left: 20px; padding-right: 20px"
      >
        <q-toolbar-title
          ><router-link to="/"><b>Where in the world?</b></router-link>
        </q-toolbar-title>

        <q-btn
          label="Dark Mode"
          no-caps
          flat
          icon="dark_mode"
          @click="darkMode"
        />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
    <footer>
      Coded by
      <a
        href="https://hectoracosta.site/"
        style="margin-left: 10px"
        :class="{
          aDark: this.$store.state.myStore.darkMode,
        }"
        >Héctor Acosta</a
      >.
    </footer>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useQuasar } from "quasar";
export default defineComponent({
  name: "MainLayout",

  methods: {
    darkMode() {
      this.$q.dark.toggle();
      this.$store.commit(
        "myStore/setDarkMode",
        !this.$store.state.myStore.darkMode
      );
    },
  },
});
</script>
