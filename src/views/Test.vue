<template>
  <div class="test">
    <h1>This is the Application page</h1>
    <div class="test-container">
      <popular/>
    </div>
  </div>
</template>

<script>
  import Popular from "../components/Popular";

  export default {
    name: 'test',
    data() {
      return {

      }
    },
    created() {

    },
    methods: {

    },
    components: {
      Popular

    },
  }
</script>

<style lang="scss" scoped>
  .gray-content {
    width: 100%;
    height: 100%;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #777777;
  }

  .test-container {
    width: 100%;
    padding-right: 15px;
    padding-left: 15px;
    margin-right: auto;
    margin-left: auto;
  }

  @media (min-width: 1200px) {
    .test-container {
      max-width: 1140px;
    }
  }

</style>
