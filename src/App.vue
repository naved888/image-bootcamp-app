<template>
  <div id="app">
    <nav-app ></nav-app>
    <upload-image-app ></upload-image-app>
    <display-image-app ></display-image-app>
    <footer-app></footer-app>
  </div>
</template>

<script>
import Nav from './components/Nav'
import UploadImage from './components/UploadImage'
import DisplayImage from './components/DisplayImage'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    "nav-app": Nav,
    "upload-image-app": UploadImage,
    "display-image-app": DisplayImage,
    "footer-app": Footer
  }
}
</script>

<style>
#app {
  font-family: 'Courier New', Courier, monospace;
}
</style>
