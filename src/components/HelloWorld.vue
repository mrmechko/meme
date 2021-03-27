import Tesseract from 'tesseract.js'

<template>
  <div class="MemeViewer">
    <h1>{{ msg }}</h1>
      <img
        :src="resolve_img_url(source)"
        alt="meme"
      >
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    source: {
      type: String,
      required: true
    }
  },
  methods: {
    resolve_img_url: function (path) {
      let images = require.context('../assets/', false, /\.png$|\.jpg$/)
      return images("./"+path)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
