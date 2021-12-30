<template>
  <div class="photo-card">
    <img :src="photo.url" class="image">
    <div class="title">{{ photo.title }}</div>
  </div>
</template>

<script>
export default {
  name: "photoCard",
  props: [
    'photo'
  ]
}
</script>

<style>
.photo-card {
  flex-direction: row;
  flex: 25%;
  width: 100%;
  padding: 25px;
}
img {
  display: flex;
  height: 280px;
  width: 280px;
}
</style>