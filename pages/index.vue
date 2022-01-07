<template>
  <div class="home">
    <!-- Search -->
    <div class="container search">
      <input
        type="text"
        placeholder="Search All Movies"
        @keyup.enter="$fetch"
        v-model.lazy="searchInput"
      />
      <button v-show="searchInput !== ''" @click="clearSearch" class="btn">
        Clear Search
      </button>

      <!-- Select -->
      <span class="select">
        <label for="page">Page:</label>
        <select name="page" v-model="pageNum">
          <option v-for="(pages, i) in this.totalPages" :key="i">
            {{ pages }}
          </option>
        </select>
        <button @click="showPage" class="btn">Go</button>
      </span>

      <!-- links -->
      <span class="links">
        <a href="#movies-list" @click="getLink('all')" class="btn"
          >View All Movies</a
        >
        <a href="#movies-list" @click="getLink('upcoming')" class="btn"
          >Upcoming Movies</a
        >
        <a href="#movies-list" @click="getLink('top')" class="btn"
          >Top Rated Movies</a
        >
        <a href="#movies-list" @click="getLink('playing')" class="btn"
          >Now Playing Movies</a
        >
      </span>
    </div>

    <!-- Loading -->
    <Loading v-if="$fetchState.pending" />

    <!-- Movies -->
    <div v-else>
      <Movies
        :apiKey="apiKey"
        :searchInput="searchInput"
        :movie="movies"
        :searchedMovies="searchedMovies"
      />
    </div>
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
      searchedMovies: [],
      searchInput: "",
      pageNum: 1,
      totalPages: 500,
      title: "All Movies",
      url: "all",
    };
  },
  async fetch() {
    this.searchInput === ""
      ? await this.getMovies()
      : ((this.pageNum = 1), this.searchMovies());
  },
  fetchDelay: 1000,
  methods: {
    getLink(value) {
      this.pageNum = 1;
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
      this.movies = [];

      const data = axios.get(
        this.url === "all"
          ? `https://api.themoviedb.org/3/discover/movie?api_key=${this.apiKey}&page=${this.pageNum}`
          : this.url === "upcoming"
          ? `https://api.themoviedb.org/3/movie/upcoming?api_key=${this.apiKey}&page=${this.pageNum}`
          : this.url === "top"
          ? `https://api.themoviedb.org/3/movie/top_rated?api_key=${this.apiKey}&page=${this.pageNum}`
          : `https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.pageNum}`
        //https://api.themoviedb.org/3/genre/movie/list?api_key=${apiKey}
      );

      const result = await data;
      result.data.results.forEach((movie) => {
        this.movies.push(movie);
      });

      result.data.total_pages > 500
        ? (this.totalPages = 500)
        : (this.totalPages = result.data.total_pages);
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
  },
};
</script>