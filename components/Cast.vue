<template>
  <div class="cast">
    <h1>Cast</h1>
    <div class="info" v-for="(cast, i) in cast" :key="i">
      <div class="profile">
        <img
          v-if="cast.profile_path"
          :src="`https://image.tmdb.org/t/p/w500/${cast.profile_path}`"
        />
        <img v-else src="../static/assets/no-img.svg" />
      </div>
      <div class="info">
        <p>{{ cast.name }}</p>
        <!-- <p>as {{ cast.character }}</p> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cast: Array,
  },
};
</script>

<style lang="scss">
.cast {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  gap: 1rem;
  padding: 32px 16px;

  h1 {
    flex: -1;
    width: 100%;
    font-size: 4rem;
    color: #f8f8ff;
  }

  .info {
    padding-bottom: 1rem;
    .profile {
      img {
        width: 160px;
        height: 240px;
      }
    }
  }
  .info {
    color: #f8f8ff;
    text-align: center;
  }
}
</style>