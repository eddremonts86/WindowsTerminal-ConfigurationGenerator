<template>
  <v-app>
    <v-app-bar app color="white" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="http://eddremonts.dk/themes/thebakerdev-zenii/assets/dist/images/logo.svg?ver1"
          transition="scale-transition"
          width="140"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn class="primary" href="https://eddremonts.dk" target="_blank" text>
        <span class="mr-2">Profile</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <HelloWorld transition="scale-transition" />
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld
  },

  data: () => ({
    //
  })
};
</script>
