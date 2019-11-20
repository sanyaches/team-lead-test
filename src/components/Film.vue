<template>
  <div
    class="film film__list__item"
    @click="emitOpenPopup(id)"
  >
    <div class="film__image">
      <img class="film__image__picture" :src="source" :alt="alt_desc">
    </div>
    <div class="film__title">
      {{ title }}
    </div>
    <div class="film__desc">
      {{ desc }}
    </div>
  </div>
</template>

<script>
  export default {
    name: "Film",
    props: ['title', 'source', 'alt_desc', 'desc', 'id', 'genres'],
    methods: {
      /**
       * Send to popup film's info by 'id'
       * @param filmId
       */
      emitOpenPopup (filmId) {
        // eslint-disable-next-line no-console
        console.log('Emit open popup for film with id:' + filmId);
        this.$emit('open-popup', filmId, this.title);
      },
    }
  }
</script>

<style lang="scss">
  .film {
    background-color: #a2a2a2;
    color: #fff;
    margin-bottom: 40px;
    padding: 5px;
    border: 1px solid wheat;
    cursor: pointer;

    &__title {
      font-family: 'Candara', sans-serif;
      font-weight: 700;
      font-size: 22px;
      margin-bottom: 20px;
    }

    &__image {
      margin-bottom: 20px;

      &__picture {
        width: 100%;
        height: auto;
        max-height: 500px;
      }
    }
    &__genres {
      font-size: 14px;
      margin-bottom: 20px;
      color: #f2f2f2;
      font-family: 'Helvetica', serif;
    }
    &__desc {
      font-family: 'Arial', sans-serif;
      font-size: 18px;
      padding: 10px;
    }
  }
</style>