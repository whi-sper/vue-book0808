<template>
  <div class="home">
    <span class="icon-bookmark"></span>
    <span class="icon-cart"></span>
  </div>
</template>


<script>
  import Epub from 'epubjs'
  global.ePub = Epub
  export default {}
</script>
<style scoped>
  .icon-bookmark {
    font-size: 20px;
    color: lightseagreen;
  }
</style>
