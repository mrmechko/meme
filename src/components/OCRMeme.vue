
<template>
  <div class="MemeViewer">
    {{ source }}
    <img
      :src=img_path
      alt="meme"
    >
    <h1>{{ ocrOutput }}</h1>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js'
import { reactive, toRefs } from "vue"
export default {
  name: 'OCRMeme',
  props: {
    source: {
      type: String,
      required: true
    }
  },
  setup(props) {
    // define the two props we are going to print from as reactive
    let computedProperties = reactive({img_path: "none", ocrOutput: "Processing..."})

    // turn img_path and ocrOutput into refs to the reactive variable. The function isn't going to wait for it to return, though
    const {img_path, ocrOutput} = toRefs(computedProperties)

    // get the path to the location where all the images are stored
    let images = require.context('../assets/', false, /\.png$|\.jpg$/)

    // compute the path to the source passed to us as a prop
    let image_path = images("./"+props.source)

    // update the ref for img_path to contain the computed image path
    computedProperties["img_path"] = image_path

    // now we are going to define a long-running function that will initialize and 
    // OCR the inputted meme
    // This function is asynchronous, meaning once it is called, the caller continues
    // on without stopping
    const ocr = (image_path) => Tesseract.recognize(
        image_path, 'eng', { logger: m => computedProperties["ocrOutput"] = m.progress }
      ).then(({data: { text } }) => { // this is a lambda!
        // We have defined an anonymous function here. This function will be run after the async function with the output of said function

        // here we are updating the ref for ocrOutput with the computed value of our ocr
        computedProperties["ocrOutput"] = text
      })
    
    // having defined the function above, run it on the image we gave it. The function isn't going to wait for it to return, though
    ocr(image_path)

    // return the reactive refs
    // we havent waited for ocr to run
    // this syntax is the equivalent of defining a dict in python like so:
    // {"img_path": img_path, "ocrOutput": ocrOutput}
    return {img_path, ocrOutput}
  },
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
