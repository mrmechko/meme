
<template>
  <div class="MemeViewer">
    <h1> {{ source }} </h1>
    <img
      :src=imgPath
      alt="meme"
    >
    <h1>{{ ocrOutput }}</h1>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js'
export default {
  name: 'OCRMeme',
  // props are passed to us from parents
  props: ["source"],
  data: function () {
    // lets set an initial value for ocrOutput
    return {
      ocrOutput: "Processing...",
    }
  },
  computed: {
    // source gives us a file name, but we need to compute the exact path on the server
    // https://vuejs.org/v2/guide/computed.html
    imgPath : function () {
      if (this.source !== "") {
        let image = require.context('../assets/', false, /\.png$|\.jpg$/)("./"+this.source)
        return image
      } 
      return ""
    }
  },
  // entries in here are called whenever the named variable changes
  watch : {
    // We want to change the ocrOutput whenever the file we are watching changes
    // https://michaelnthiessen.com/how-to-watch-nested-data-vue/
    imgPath: {
      // The first time imgPath is computed doesn't count as a "change" so we need to specify immediate
      immediate: true,
      // this is the function to run when there is a change. if the function had two arguments, we could compare the new value to the previous value too
      handler(newPath) {
        // call our OCR api. This is an asynchronous function so we need to have a "callback" for when it finishes
        Tesseract.recognize(
          newPath, 'eng', { logger: m => this.ocrOutput = m.progress }
        ).then(({data: { text } }) => { // this is a lambda!
          // We have defined an anonymous function here. This function will be run after the async function with the output of said function
          // here we are updating the ref for ocrOutput with the computed value of our ocr
          this.ocrOutput = text
        })
      }
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
