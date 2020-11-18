<template>
  <v-card :loading="is_loading" elevation="8" class="d-inline-block ma-5" :color="song.color" height="150px" right width="350px">
    <div class="d-flex flex-no-wrap justify-space-between">
      <div style="position:relative;">
        <v-card-title class="text-sm-body-2 font-weight-bold d-inline-block text-truncate" style="width: 200px;" v-text="song.name"></v-card-title>

        <v-card-subtitle class="d-inline-block text-truncate" style="max-width: 200px;">
          <span class="d-inline-block">{{ song.album }}</span
          ><br />
          <span class="d-inline-block">{{ song.artist }}</span>
        </v-card-subtitle>

        <v-btn icon style="position:absolute;left:10px;bottom:10px;" height="40px" right width="40px" @click="playThis">
          <v-icon>{{ playingThis }}</v-icon>
        </v-btn>
      </div>

      <v-avatar class="ma-3" size="125" tile>
        <v-img :src="album_art()" @load="is_loading = false"></v-img>
      </v-avatar>
    </div>
  </v-card>
</template>
<script>
export default {
  props: {
    song: Object
  },
  data: () => ({
    is_loading: true,
    playing: null
  }),
  methods: {
    album_art() {
      return `https://cdn.asterian.dev/Song/${this.song.artist}/${this.song.album}/cover.png`;
    },
    playThis() {
      if (this.playing === null) {
        this.$root.$emit("playThis", this.song);
        this.$root.$emit("playingSong", this.song);
      } else {
        if (this.playing.id === this.song.id) {
          this.$root.$emit("StopSong");
        } else {
          this.$root.$emit("playThis", this.song);
          this.$root.$emit("playingSong", this.song);
        }
      }
    }
  },
  computed: {
    playingThis() {
      if (this.playing === null) {
        return "mdi-play";
      }
      return this.song.id === this.playing.id ? "mdi-pause" : "mdi-play";
    }
  },
  mounted() {
    this.$root.$on("playingSong", song => {
      this.playing = song;
    });
  }
};
</script>
