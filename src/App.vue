<template>
  <main id="app">
    <div class="top-bar">
      <h1>Photo Gallery</h1>
      <div class="action-container">
        <input placeholder="Search" v-model="inputSearch" v-on:keyup.enter=searchPhoto>
        <button @click=searchPhoto><img alt="Buscar foto" class="icon" src="./assets/search.png"></button>
        <button @click="gridView = true"><img alt="Visualização em grade" class="icon" src="./assets/grid.png"></button>
        <button @click="gridView = false"><img alt="Visualização em lista" class="icon" src="./assets/list.png"></button>
      </div>
    </div>
    <div :class=currentView>
      <div :class=currentPhotoView v-for="(photo, index) in renderedPhotos" :key=index>
        <img :src="photo.url" :class=currentViewImage>
        <div :class=currentViewTitle>{{ photo.title }}</div>
      </div>
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
      gridView: 'grid',
      inputSearch: null,
      searchOn: false,
      filtered: []
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
    searchPhoto: function () {
      this.searchOn = true
      this.filtered = this.photos.filter(({title}) => title.indexOf(this.inputSearch) >= 0);
    },
    showMore: function () {
      this.lastEl += 20;
    },
  },
  computed: {
    renderedPhotos: function() {
      if(!this.searchOn)
        return this.photos.slice(0, this.lastEl)
      if(this.searchOn)
        return this.filtered.slice(0, this.lastEl)
    },
    currentView: function () {
      if(this.gridView)
        return 'grid'
      if(!this.gridView)
        return 'list'
    },
    currentPhotoView: function () {
      if(this.gridView)
        return 'photo-card'
      if(!this.gridView)
        return 'photo-list'
    },
    currentViewImage: function () {
      if(this.gridView)
        return 'image-grid'
      if(!this.gridView)
        return 'image-list'
    },
    currentViewTitle: function () {
      if(this.gridView)
        return 'title-grid'
      if(!this.gridView)
        return 'title-list'
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
  text-align: center;
}
.top-bar {
  padding-top: 25px;
  background: lightgray;
  justify-content: space-around;
}
.top-bar h1 {
  display: inline;
  margin: auto;
  vertical-align: middle;
  font-size: 38px;
}
.top-bar input {
  height: 30px;
  width: 200px;
  vertical-align: middle;
}
.action-container {
  display: inline;
  margin: auto auto auto 25px;
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
  padding: 5px;
  margin: 0 auto;
}
.list {
  display: inline;
  text-align: left;
}
.photo-card {
  margin: auto;
  flex-direction: row;
  flex: 15%;
  width: 100%;
  padding: 25px;
}
.photo-list{
  margin: auto;
  width: 720px;
  padding: 15px;
}
.image-grid {
  display: flex;
  margin: auto;
  height: 280px;
  width: 280px;
  border-radius: 5px;
}
.image-list {
  display: inline;
  vertical-align: middle;
  margin: 15px auto;
  height: 150px;
  width: 150px;
  border-radius: 5px;
}
.title-grid {
  margin: 15px auto;
}
.title-list {
  display: inline-block;
  vertical-align: middle;
  max-width: 180px;
  overflow-x: hidden;
  margin-left: 15px;
}
.show-more {
  margin: 10px auto 20px auto;
  padding: 5px;
}
</style>
