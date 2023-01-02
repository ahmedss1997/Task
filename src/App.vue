<template>
  <v-app>
    <v-app-bar app color="primary" dark height="80" class="mb-4">
      <v-container fluid>
        <v-row>
          <v-col cols="6"  class="d-flex align-center">
            <div class="d-flex align-center">
              <v-img
                alt="Vuetify Logo"
                class="shrink mr-2"
                contain
                src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
                transition="scale-transition"
                width="40"
              />
              <v-img
                alt="Vuetify Name"
                class="shrink mt-1 hidden-sm-and-down"
                contain
                min-width="100"
                src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
                width="100"
              />
            </div>
          </v-col>
          <v-spacer></v-spacer>
          <v-col
            class="d-flex align-center justify-end"
            cols="6"
          >
            <v-sheet class="up" width="250">
              <!-- <v-select
                light
                :items="items"
                label="choose languages"
                hide-details
                solo
              >
                <template v-slot:prepend-item>
                  <v-list-item class="v-list-item--link">
                    <v-img src="../src/assets/imgs/united.jpeg" max-width="30" class="mr-2"></v-img>
                    <v-list-item-title>English</v-list-item-title>
                  </v-list-item>
                </template>
              </v-select> -->
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
      
    </v-app-bar>
    <v-main class="bgColor pt-15 transition-all duration-500 ease-in-out">
      <router-view />
    </v-main>
  </v-app>
</template>

<script lang="ts">

import Vue from "vue";

export default Vue.extend({
  name: "App",
  data: () => ({
    items: ['Foo', 'Bar', 'Fizz', 'Buzz'],
  }),
});

</script>

<style lang="scss">

// .v-menu__content {
//   top: 53px !important;
//   border-radius: 0px !important;
// }

</style>
