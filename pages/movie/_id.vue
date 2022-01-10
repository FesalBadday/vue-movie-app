<template>
  <!-- Loading -->
  <Loading v-if="$fetchState.pending" />

  <!-- Movie Info -->
  <div v-else>
    <Details :movie="movie" :trailer="trailer" />
    <Cast :cast="casts" />
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "movie",
  async fetch() {
    await this.getMovie();
  },
  fetchDelay: 1000,
  head() {
    return {
      title: this.movie.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.movie.overview,
        },
      ],
    };
  },
  data() {
    return {
      //apiKey: process.env.VUE_APP_API_KEY,
      movie: {},
      trailer: "",
      casts: [],
    };
  },
  methods: {
    async getMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=${this.$route.params.apiKey}`
      );
      const result = await data;
      this.movie = result.data;
      this.getCast();
      this.getTrailer();
    },
    async getCast() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}/credits?api_key=${this.$route.params.apiKey}`
      );
      const result = await data;
      result.data.cast.forEach((cast) => {
        this.casts.push(cast);
      });
    },
    async getTrailer() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}/videos?api_key=${this.$route.params.apiKey}`
      );
      const result = await data;
      result.data.results.forEach((video) => {
        if (video.type === "Trailer") {
          this.trailer = video.key;
        }
      });
    },
  },
};
</script>
