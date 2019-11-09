<template>
  <figure id="nasa-picture">
    <img :src="picture.url" alt="picture.title">
    <figcaption>{{picture.title}}, {{picture.date}}</figcaption>
  </figure>
</template>

<script>
  export default {
    name: 'nasa-picture',
    props: {
      picture: Object,
    }
  }
</script>

<style scoped>
  img {
    max-height: 250px;
    margin-top: 5px;
    border: 1px solid #403e37;
  }
</style>
