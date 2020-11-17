<template>
  <v-bottom-navigation fixed v-model="value" :background-color="color" dark shift>
    <v-btn>
      <span>Playing</span>

      <v-icon>mdi-music-note</v-icon>
    </v-btn>
    <v-btn>
      <span>Songs</span>

      <v-icon>mdi-playlist-music</v-icon>
    </v-btn>

    <v-btn>
      <span>Favourite</span>

      <v-icon>mdi-heart</v-icon>
    </v-btn>

    <v-btn>
      <span>Albums</span>

      <v-icon>mdi-album</v-icon>
    </v-btn>
  </v-bottom-navigation>
</template>
<script>
export default {
  data: () => ({ value: 0 }),

  computed: {
    color() {
      switch (this.value) {
        case 0:
          return "blue-grey";
        case 1:
          return "teal";
        case 2:
          return "red";
        case 3:
          return "indigo";
        default:
          return "blue-grey";
      }
    }
  }
};
</script>
