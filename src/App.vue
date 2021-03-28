<template>
  <OCRMeme source="one.png"/>
</template>

<script>
import OCRMeme from './components/OCRMeme.vue'

export default {
  name: 'App',
  components: {
    OCRMeme
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
