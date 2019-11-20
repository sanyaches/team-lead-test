<template>
  <div class="film film__list__item">
    <div class="film__title">
      {{ title }}
    </div>
  </div>
</template>

<script>
  export default {
    name: "Film",
    props: ['title',]
  }
</script>

<style lang="scss">
  .film {
    margin-bottom: 30px;
    &__title {
      font-size: 20px;
    }
  }
</style>