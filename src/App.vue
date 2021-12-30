<template>
  <main id="app">
    <section class="action-container">
      <h1>Photo Gallery</h1>
      <input placeholder="Search">
    </section>
    <div class="gallery">
      <photoCard
        v-for="photo in renderedPhotos"
        :key=photo.title
        :photo='photo'
      />
      <button @click="showMore">SHOW MORE</button>
    </div>
  </main>
</template>

<script>
import axios from "axios"
import photoCard from './components/photoCard.vue'

export default {
  name: "App",
  components: {
    photoCard
  },
  data() {
    return {
      photos: [],
      lastEl: 20,
    }
  },
  methods: {
    getPhotos: function () {
      axios.
        get("https://jsonplaceholder.typicode.com/photos")
          .then(response => {
            this.photos = response.data
          })
          .catch(error => {
            alert(error)
          }) 
    },
    showMore: function () {
      this.lastEl += 20;
    }
  },
  computed: {
    renderedPhotos: function() {
      return this.photos.slice(0, this.lastEl)
    }
  },
  beforeMount() {
    this.getPhotos();
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Roboto", sans-serif;
}
.action-container h1 {
  display: inline;
  margin: 25px auto;
  padding: 25px;
}
.action-container input {
  display: inline;
  margin: 25px auto;
  height: 30px;
}
.main {
  width: 100vw;
  min-height: 100vw;
  overflow: hidden;
  justify-content: space-between;
  display: flex;
}
.gallery {
  display: flex;
  flex-wrap: wrap;
  padding: 25px;
  margin: 0 auto;
}
.show-more {
  text-align: center;
}
</style>
