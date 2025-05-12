<template>
  <PDF_Upload />
</template>

<script>
import PDF_Upload from './components/PDF_Upload.vue'

export default {
  name: 'App',
  components: {
    PDF_Upload
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
