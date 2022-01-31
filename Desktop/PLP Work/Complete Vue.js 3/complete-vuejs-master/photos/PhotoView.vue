<template>
  <img 
    v-for="photo in photosForCurrentAlbum" 
    :key="photo.thumbnailUrl" 
    :src="photo.thumbnailUrl"
  /> 
</template>

<script>
import { useStore } from 'vuex'
import { computed } from 'vue'
//so this is where i had started by renaming photos to photosForCurrentAlbum and had yet to updated PhotoApp.vue
//also, was pulling in the wrong key in my template, was pulling album.id instead
export default {
  setup() {
    const store = useStore()
    const photosForCurrentAlbum = computed(() => {
      return store.state.photos.photosForCurrentAlbum
    })
    return {
      photosForCurrentAlbum
    }
  }
}
</script>

<style scoped>
img {
  margin: 5px;
}
</style>