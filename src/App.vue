<template>
  <v-app>
    <Nav />
    <MusicPlayer />
    <MenuDrawer />
    <v-main>
      <audio class="audioPlayer d-none" @canplaythrough="startPlaying" @ended="nextSong"></audio>
      <div class="d-flex justify-center flex-wrap mb-15" v-if="songs.length != 0">
        <AlbumCard v-for="song in songs" :song="song" :key="song.id" />
      </div>
      <div class="d-flex justify-center flex-wrap mb-15" v-if="songs.length == 0">
        <Loader :amount="song_amount" />
      </div>
    </v-main>
    <BottomMenu />
  </v-app>
</template>

<script>
import Nav from "./components/Nav.vue";
import AlbumCard from "./components/AlbumCard.vue";
import BottomMenu from "./components/BottomMenu.vue";
import MusicPlayer from "./components/MusicPlayer.vue";
import Loader from "./components/Loader.vue";
import MenuDrawer from "./components/MenuDrawer.vue";

const axios = require("axios");
const _ = require("lodash");

export default {
  name: "App",

  components: {
    Nav,
    AlbumCard,
    BottomMenu,
    MusicPlayer,
    Loader,
    MenuDrawer
  },

  data: () => ({
    songs: [],
    song_amount: 51,
    playing_song_info: Object
  }),
  methods: {
    stopSong() {
      const audioPlayer = document.querySelector(".audioPlayer");
      audioPlayer.pause();
    },
    resumeSong() {
      const audioPlayer = document.querySelector(".audioPlayer");
      audioPlayer.play();
    },
    startSong(song) {
      this.playing_song_info = song;
      const audioPlayer = document.querySelector(".audioPlayer");
      audioPlayer.src = this.getSrc(song);
      this.$root.$emit("songLoadingStart");
    },
    getSrc(song) {
      return `https://cdn.asterian.dev/Song/${song.artist}/${song.album}/${song.name}.mp3`;
    },
    startPlaying() {
      const audioPlayer = document.querySelector(".audioPlayer");
      audioPlayer.play();
      this.$root.$emit("songLoadingEnd");
    },
    nextSong() {
      const nextSongInfo = this.songs[this.songs.indexOf(this.playing_song_info) + 1];
      this.startSong(nextSongInfo);
      this.$root.$emit("playThis", nextSongInfo);
      this.$root.$emit("playingSong", nextSongInfo);
    }
  },
  created() {
    axios
      .post("https://unlimited-song.herokuapp.com/api/data", {
        apikey: "0j6VZnguTZ30MHKsBMw2UEBrNicv8cZA"
      })
      .then(({ data }) => {
        this.songs = _.sampleSize(data.song_data, this.song_amount);
      });
  },
  mounted() {
    this.$root.$on("StopSong", this.stopSong);
    this.$root.$on("ResumeSong", this.resumeSong);
    this.$root.$on("playThis", this.startSong);
    this.$root.$on("skip10s", () => {
      document.querySelector(".audioPlayer").currentTime += 10;
    });
    this.$root.$on("prev10s", () => {
      document.querySelector(".audioPlayer").currentTime -= 10;
    });
  }
};
</script>
