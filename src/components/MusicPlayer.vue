<template>
  <div class="text-center">
    <v-bottom-sheet inset v-model="sheet">
      <v-card tile v-if="song_from_card != null">
        <v-progress-linear :indeterminate="songLoading" :buffer-value="currentTimeProgress" class="my-0" height="3"></v-progress-linear>
        <v-list>
          <v-list-item>
            <v-list-item-avatar>
              <v-img :src="album_art()"></v-img>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title>{{ song_from_card.name }}</v-list-item-title>
              <v-list-item-subtitle>{{ song_from_card.artist }}</v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-content>
              <v-list-item-subtitle>{{ current_time }} &emsp;|&emsp; {{ total_time }}</v-list-item-subtitle>
            </v-list-item-content>

            <v-spacer></v-spacer>

            <v-list-item-icon>
              <v-btn icon @click="prev10s">
                <v-icon>mdi-rewind</v-icon>
              </v-btn>
            </v-list-item-icon>

            <v-list-item-icon :class="{ 'mx-5': $vuetify.breakpoint.mdAndUp }">
              <v-btn icon @click="pause">
                <v-icon>{{ ispause ? "mdi-play" : "mdi-pause" }}</v-icon>
              </v-btn>
            </v-list-item-icon>

            <v-list-item-icon class="ml-0" :class="{ 'mr-3': $vuetify.breakpoint.mdAndUp }">
              <v-btn icon @click="skip10s">
                <v-icon>mdi-fast-forward</v-icon>
              </v-btn>
            </v-list-item-icon>
          </v-list-item>
        </v-list>
      </v-card>
    </v-bottom-sheet>
  </div>
</template>
<script>
const getColors = require("get-image-colors");

export default {
  data: () => ({
    value: 0,
    sheet: false,
    song_from_card: null,
    songLoading: true,
    currentTimeProgress: 0,
    ispause: false,
    current_time: "00:00",
    total_time: "00:00"
  }),
  methods: {
    album_art() {
      return `https://cdn.asterian.dev/Song/${this.song_from_card.artist}/${this.song_from_card.album}/cover.png`;
    },
    skip10s() {
      this.$root.$emit("skip10s");
    },
    prev10s() {
      this.$root.$emit("prev10s");
    },
    pause() {
      if (this.ispause) {
        this.$root.$emit("ResumeSong");
      } else {
        this.ispause = true;
        this.$root.$emit("StopSong");
      }
      getColors(this.album_art(), "image/gif").then(colors => {
        console.log(colors);
      });
    },
    watch_audioPlayer() {
      const audioPlayer = document.querySelector(".audioPlayer");

      audioPlayer.addEventListener("timeupdate", () => {
        this.current_time = this.timeFormat(audioPlayer.currentTime);
        this.total_time = this.timeFormat(audioPlayer.duration);
        this.currentTimeProgress = (audioPlayer.currentTime / audioPlayer.duration) * 100;
        this.ispause = audioPlayer.paused;
      });
    },
    timeFormat(seconds) {
      const m = Math.floor(seconds / 60) < 10 ? `0${Math.floor(seconds / 60)}` : Math.floor(seconds / 60);
      const s = Math.floor(seconds - m * 60) < 10 ? `0${Math.floor(seconds - m * 60)}` : Math.floor(seconds - m * 60);
      const toReturn = `${m}:${s}`;
      if (toReturn === "NaN:NaN") {
        return "00:00";
      }
      return toReturn;
    }
  },
  watch: {
    sheet(item) {
      if (!item) {
        this.$root.$emit("BottomMenuClosed");
      }
    }
  },
  mounted() {
    this.$root.$on("playThis", song => {
      this.song_from_card = song;
      this.sheet = !this.sheet;
    });
    this.$root.$on("openMusicPlayer", () => {
      if (this.song_from_card != null) {
        this.sheet = true;
      }
    });
    this.$root.$on("songLoadingStart", () => {
      if (this.song_from_card != null) {
        this.songLoading = true;
      }
    });
    this.$root.$on("songLoadingEnd", () => {
      if (this.song_from_card != null) {
        this.songLoading = false;
      }
    });
    this.watch_audioPlayer();
  }
};
</script>
