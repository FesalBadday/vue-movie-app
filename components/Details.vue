<template>
  <div class="container movie-details">
    <!-- <a href="/" class="btn">Previous Page</a> -->
    <NuxtLink class="btn" :to="{ name: 'index' }">Back</NuxtLink>
    <div class="info">
      <div class="poster">
        <img
          v-if="movie.poster_path"
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          alt=""
        />
        <img v-else src="../static/assets/no-img.svg" />
      </div>
      <div class="details">
        <h1>{{ movie.title }}</h1>
        <h2>{{ movie.vote_average }}</h2>
        <p class="facts tagline">
          <span v-for="(category, i) in movie.genres" :key="i"
            ><span v-if="i === 0">Category: </span>{{ category.name
            }}<span v-if="i !== movie.genres.length - 1">, </span></span
          >
        </p>
        <p class="facts">
          <span>Date Released:</span>
          {{
            new Date(movie.release_date).toLocaleString("en-us", {
              month: "long",
              day: "numeric",
              year: "numeric",
            })
          }}
        </p>
        <p class="facts">
          <span>Duration:</span>
          {{
            Math.floor(movie.runtime / 60) + "h " + (movie.runtime % 60) + "m"
          }}
        </p>
        <p class="facts"><span>Overview:</span> {{ movie.overview }}</p>
        <div class="trailer">
          <iframe :src="`https://www.youtube.com/embed/${trailer}`"></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    movie: Object,
    trailer: String,
  },
};
</script>

<style lang="scss">
.movie-details {
  color: #f8f8ff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .btn {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #f8f8ff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .poster {
      img {
        width: 100%;
        @media (min-width: 800px) {
          width: initial;
        }
      }
    }
    .details {
      h1 {
        font-size: 3rem;
      }
      h2 {
        background-color: #008000;
        width: fit-content;
        padding: 0.5rem;
        border-radius: 0.5rem;
        margin: 2rem 0;
      }
      .facts {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
      }
      .trailer {
        padding-top: 2rem;
        iframe {
          width: 100%;
          height: 600px;
          border: none;
        }
      }
    }
  }
}
</style>