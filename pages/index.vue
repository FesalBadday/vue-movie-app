<template>
  <div class="main">
    <div class="menu">
      <input
        type="text"
        placeholder="Search All Movies"
        @keyup.enter="$fetch"
        v-model.lazy="searchInput"
      />
      <button v-show="searchInput !== ''" @click="clearSearch" class="btn">
        Clear
      </button>

      <!-- Select -->
      <div class="select">
        <label for="page">Page:</label>
        <select name="page" v-model="pageNum" @change="showPage">
          <option v-for="(pages, i) in this.totalPages" :key="i">
            {{ pages }}
          </option>
        </select>
      </div>

      <Links :getLink="getLink" :showGenre="showGenre" :genre="genres" />
    </div>

    <!-- Loading -->
    <Loading v-if="$fetchState.pending" />

    <!-- Movies -->
    <Movies
      v-else
      :searchInput="searchInput"
      :apiKey="apiKey"
      :movie="movies"
      :searchedMovies="searchedMovies"
    />
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "index",
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Movies API App",
        },
      ],
    };
  },
  data() {
    return {
      apiKey: process.env.VUE_APP_API_KEY,
      movies: [],
      genres: [],
      searchedMovies: [],
      genre: "",
      searchInput: "",
      pageNum: 1,
      totalPages: 500,
      title: "All Movies",
      url: "all",
    };
  },
  async fetch() {
    await this.getGenres();
    this.searchInput === ""
      ? await this.getMovies()
      : ((this.pageNum = 1), this.searchMovies());
  },
  fetchDelay: 1000,
  methods: {
    getLink(value) {
      this.pageNum = 1;
      this.genre = "";
      this.url = value;
      this.getMovies();
      this.clearSearch();

      this.url === "upcoming"
        ? (this.title = "Upcoming Movies")
        : this.url === "top"
        ? (this.title = "Top Rated Movies")
        : this.url === "playing"
        ? (this.title = "Now Playing Movies")
        : (this.title = "All Movies");
    },
    async getMovies() {
      const data = axios.get(
        this.url === "all"
          ? `https://api.themoviedb.org/3/discover/movie?api_key=${this.apiKey}&page=${this.pageNum}&with_genres=${this.genre}`
          : `https://api.themoviedb.org/3/movie/${this.url}?api_key=${this.apiKey}&page=${this.pageNum}`
      );

      const result = await data;
      this.movies = [];
      result.data.results.forEach((movie) => {
        this.movies.push(movie);
      });

      result.data.total_pages > 500
        ? (this.totalPages = 500)
        : (this.totalPages = result.data.total_pages);
    },
    async getGenres() {
      this.genres = [];

      const data = axios.get(
        `https://api.themoviedb.org/3/genre/movie/list?api_key=${this.apiKey}`
      );

      const result = await data;
      result.data.genres.forEach((genre) => {
        this.genres.push(genre);
      });
    },
    async searchMovies() {
      this.searchedMovies = [];
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=${this.apiKey}&query=${this.searchInput}&page=${this.pageNum}`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie);
      });
      this.totalPages = result.data.total_pages;
    },
    clearSearch() {
      this.searchInput = "";
      this.searchedMovies = [];
      this.pageNum = 1;
      this.getMovies();
    },
    showPage() {
      this.searchedMovies.length === 0 ? this.getMovies() : this.searchMovies();
    },
    showGenre(value) {
      this.url = "all";
      this.genre = value;
      this.clearSearch();
      this.getMovies();
    },
  },
};
</script>
