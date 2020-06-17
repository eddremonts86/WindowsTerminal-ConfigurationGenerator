<template>
  <v-app>
    <v-app-bar app color="white" dark flat>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          contain
          src="http://eddremonts.dk/themes/thebakerdev-zenii/assets/dist/images/logo.svg?ver1"
          width="140"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn text color="blue darken-4" href="/">
        <span>Home</span>
      </v-btn>

      <v-btn text color="blue darken-4" href="/generator">
        <span>Generaor</span>
      </v-btn>
      <v-btn
        text
        color="blue darken-4"
        href="https://eddremonts.dk"
        target="_blank"
      >
        <span class="mr-2">About me</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
    <v-content>
      <router-view transition="scale-transition" />
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    //
  })
};
</script>
<style>
@media (min-width: 1904px) {
  .container {
    max-width: 1200px !important;
  }
}
</style>
