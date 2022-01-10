<template>
  <!-- Links -->
  <div class="links">
    <a href="#movies-list" @click="getLink('all')" class="btn">All</a>
    <a href="#movies-list" @click="getLink('upcoming')" class="btn">Upcoming</a>
    <a href="#movies-list" @click="getLink('top_rated')" class="btn"
      >Top Rated</a
    >
    <a href="#movies-list" @click="getLink('now_playing')" class="btn"
      >Now Playing</a
    >
    <!-- Genres -->
    <div class="genres" v-for="(genre, i) in genre" :key="i">
      <a href="#" @click="showGenre(genre.id)" class="btn">{{ genre.name }}</a>
      <!-- <NuxtLink class="btn" :to="{ name: 'index' }">Back</NuxtLink> -->
    </div>
  </div>
</template>

<script>
export default {
  props: {
    getLink: Function,
    showGenre: Function,
    genre: Array,
  },
};
</script>
