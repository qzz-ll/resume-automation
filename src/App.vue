<template>
  <div id="app">
    <index />
  </div>
</template>

<script>
import index from './components/index/index.vue'

export default {
  name: 'App',
  components: {
    index
  }
}
</script>
<style>
@import "public/css/init.css";
  html{
    font-size: 62.5%;
    background: #efefef;
  }
</style>
