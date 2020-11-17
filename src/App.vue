<template>
  <v-app>
    <Nav />
    <v-main>
      <div class="d-flex justify-center flex-wrap">
        <AlbumCard v-for="song in songs" :song="song" :key="song.id" />
      </div>
    </v-main>
    <BottomMenu />
  </v-app>
</template>

<script>
import Nav from "./components/Nav.vue";
import AlbumCard from "./components/AlbumCard.vue";
import BottomMenu from "./components/BottomMenu.vue";

const axios = require("axios");
const _ = require("lodash");

export default {
  name: "App",

  components: {
    Nav,
    AlbumCard,
    BottomMenu
  },

  data: () => ({
    songs: Array
  }),
  created() {
    axios.get("https://unlimited-song.herokuapp.com/data").then(({ data }) => {
      this.songs = _.sampleSize(data.song_data, 5);
    });
  }
};
</script>
