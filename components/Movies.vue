<template>
  <div class="container">
    <!-- Searched Movies  -->
    <div id="movies-list" v-if="searchInput !== ''" class="movies-list">
      <div class="movie" v-for="(movie, i) in searchedMovies" :key="i">
        <div class="poster">
          <img
            v-if="movie.poster_path"
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            alt=""
          />
          <img
            v-else
            src="../static/assets/no-img.svg"
          />
          <p class="review">{{ movie.vote_average }}</p>
          <p class="overview">{{ movie.overview }}</p>
        </div>
        <div class="info">
          <NuxtLink
            class="btn"
            :to="{
              name: 'movie-id',
              params: { id: movie.id, apiKey: apiKey },
            }"
          >
            More Info
          </NuxtLink>
        </div>
      </div>
    </div>

    <!-- Movies  -->
    <div id="movies-list" v-else class="movies-list">
      <div class="movie" v-for="(movie, i) in movie" :key="i">
        <div class="poster">
          <img
            v-if="movie.poster_path"
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            alt=""
          />
          <img
            v-else
            src="../static/assets/no-img.svg"
          />
          <p class="review">{{ movie.vote_average }}</p>
          <p class="overview">{{ movie.overview }}</p>
        </div>
        <div class="info">
          <NuxtLink
            class="btn"
            :to="{
              name: 'movie-id',
              params: { id: movie.id, apiKey: apiKey },
            }"
          >
            More Info
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    searchInput: String,
    apiKey: String,
    movie: Array,
    searchedMovies: Array,
  },
};
</script>

<style lang="scss">
.main {
  display: flex;
  background-color: #222222;
  padding: 2rem 1rem;

  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }

  .menu {
    flex: 0.2;
    width: 100%;
    padding-right: 1rem;
    input {
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }

    .select {
      color: #f8f8ff;
      padding: 1rem 0;
      select {
        padding: 6px 6px;
        width: 100%;
      }
    }

    .links,
    .genres {
      display: flex;
      flex-direction: column;
    }
  }

  .container {
    flex: 0.8;

    .movies-list {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .poster {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 1px;
            left: 1px;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 2.4rem;
            height: 2.4rem;
            background-color: #000;
            color: #f8f8ff;
            border-radius: 50%;
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: #000;
            padding: 12px;
            color: #f8f8ff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
          }
        }
        .info {
          .btn {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>