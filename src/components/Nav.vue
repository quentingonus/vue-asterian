<template>
  <v-card>
    <v-app-bar elevate-on-scroll>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Asterian</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn
        icon
        transition="scale-transition"
        :loading="loading"
        @click="change_modes"
        class="mr-5"
        ><v-icon>{{ day_night }}</v-icon></v-btn
      >
    </v-app-bar>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      loading: false
    };
  },
  computed: {
    day_night() {
      this.make_loading();
      return this.$vuetify.theme.dark ? "mdi-moon-waxing-crescent" : "mdi-white-balance-sunny";
    }
  },
  methods: {
    change_modes() {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
    },
    make_loading() {
      this.loading = true;
      setTimeout(() => {
        this.loading = !this.loading;
      }, Math.floor(Math.random() * 500) + 1);
    }
  }
};
</script>
