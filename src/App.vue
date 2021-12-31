<template>
  <main id="app">
    <div class="top-bar">
      <h1>Photo Gallery</h1>
      <div class="action-container">
        <input placeholder="Search">
        <button @click="gridView = true"><img alt="Visualização em grade" class="icon" src="./assets/grid.png"></button>
        <button @click="gridView = false"><img alt="Visualização em lista" class="icon" src="./assets/list.png"></button>
      </div>
    </div>
    <div v-for="(photo, index) in renderedPhotos" :key=index>
      <img :src="photo.url" class="image-grid">
      <div class="title">{{ photo.title }}</div>
    </div>
    <button class="show-more" @click="showMore">SHOW MORE</button>
  </main>
</template>

<script>
import axios from "axios"

export default {
  name: "App",
  data() {
    return {
      photos: [],
      lastEl: 20,
      gridView: 'grid'
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
    },
    currentViewImage: function () {
      if(view === 'grid')
        return 'image-grid'
      if(view === 'list')
        return  'image-list'
    }
  },
  beforeMount() {
    this.getPhotos();
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}
.top-bar {
  justify-content: space-around;
}
.top-bar h1 {
  display: inline;
  margin-left: 45px;
}
.top-bar input {
  margin: 25px auto;
  height: 30px;
  width: 200px;
}
.action-container {
  display: inline;
  margin-left: 45px;
}
.action-container button {
  margin: 15px;
  border: none;
  background: none;
  cursor: pointer;
  vertical-align: middle;
}
.icon {
  height: 40px;
  width: 40px;
}
.main {
  width: 100%;
  overflow: hidden;
  justify-content: space-between;
  text-align: center;
}
.grid {
  display: flex;
  flex-wrap: wrap;
  padding: 25px;
  margin: 0 auto;
}
.list {
  display: inline;
}
.photo-card {
  flex-direction: row;
  flex: 25%;
  width: 100%;
  padding: 25px;
}
.image-grid {
  display: flex;
  margin: auto;
  height: 280px;
  width: 280px;
}
.title {
  margin: auto;
  text-align: center;
}
.show-more {
  text-align: center;
}
</style>
