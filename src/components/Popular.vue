<template>
  <div class="popular-films films">
    <h2 class="films__title">Популярные фильмы</h2>

    <div class="films__list">
      <film
        v-for="(film, index) in popularFilms"
        :key="index"
        :title="film.title"
      />
    </div>
  </div>
</template>

<script>
  import Film from "./Film";
  export default {
    name: "Popular",
    components: {Film},

    created() {
      axios()
    },

    data () {
      return {
        popularFilms: [],
      }
    }
  }
</script>

<style lang="scss">
  .films {
    padding: 60px 0;
    &__list {
      margin-top: 60px;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-column-gap: 30px;

      &__item {

      }
    }
  }
</style>