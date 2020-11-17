<template>
  <v-bottom-navigation fixed v-model="value" :background-color="color" dark shift>
    <v-btn @click="openMusicPlayer">
      <span>Playing</span>

      <v-icon>mdi-music-note</v-icon>
    </v-btn>
    <v-btn @click="openPlaylist">
      <span>Songs</span>

      <v-icon>mdi-playlist-music</v-icon>
    </v-btn>

    <v-btn @click="openFav">
      <span>Favourite</span>

      <v-icon>mdi-heart</v-icon>
    </v-btn>

    <v-btn @click="openAlbum">
      <span>Albums</span>

      <v-icon>mdi-album</v-icon>
    </v-btn>
  </v-bottom-navigation>
</template>
<script>
export default {
  data: () => ({
    value: 1,
    pre_value: 1
  }),
  methods: {
    openMusicPlayer() {
      this.$root.$emit("openMusicPlayer");
    },
    openPlaylist() {
      this.pre_value = 1;
    },
    openFav() {
      this.pre_value = 2;
    },
    openAlbum() {
      this.pre_value = 3;
    }
  },
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
  },
  mounted() {
    this.$root.$on("BottomMenuClosed", () => {
      this.value = this.pre_value;
    });
  }
};
</script>
