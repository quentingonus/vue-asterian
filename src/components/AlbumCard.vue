<template>
  <v-card
    :loading="false"
    class="d-inline-block ma-5"
    :color="song.color"
    height="150px"
    right
    width="350px"
  >
    <div class="d-flex flex-no-wrap justify-space-between">
      <div style="position:relative;">
        <v-card-title
          class="text-sm-body-2 font-weight-bold d-inline-block text-truncate"
          style="width: 200px;"
          v-text="song.name"
        ></v-card-title>

        <v-card-subtitle class="d-inline-block text-truncate" style="max-width: 200px;">
          <span class="d-inline-block">{{ song.album }}</span
          ><br />
          <span class="mt-1 d-inline-block">{{ song.artist }}</span>
        </v-card-subtitle>

        <v-btn
          icon
          style="position:absolute;left:10px;bottom:10px;"
          height="40px"
          right
          width="40px"
        >
          <v-icon>mdi-play</v-icon>
        </v-btn>
      </div>

      <v-avatar class="ma-3" size="125" tile>
        <v-img :src="album_art()"></v-img>
      </v-avatar>
    </div>
  </v-card>
</template>
<script>
export default {
  props: {
    song: Object
  },
  data: () => ({}),
  methods: {
    album_art() {
      return `https://cdn.asterian.dev/Song/${this.song.artist}/${this.song.album}/cover.png`;
    }
  }
};
</script>
