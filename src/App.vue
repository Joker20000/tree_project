<template>
  <div id="app">
    <tree></tree>
  </div>
</template>
<script>
  import tree from './components/tree.vue'
  export default {
      name: 'app',

      components: {
          tree
      }
  }
</script>
<style>
  body{
    margin: 0;
  }
</style>