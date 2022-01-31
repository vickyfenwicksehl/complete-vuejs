<template>
  <layout>
    <template v-slot:header>
      Capstone Project - Photo App
    </template>

    <template v-slot:sidebar>
      <album 
        v-for="album in albums" 
        :key="album.id"
        :album="album"
      />
    </template>

    <template v-slot:content>
      <router-view />
    </template>
  </layout>
</template>

<script>
import { onBeforeMount, computed, watch } from 'vue'
import { useRoute } from 'vue-router'
import { useStore } from 'vuex'
import Layout from './Layout.vue'
import Album from './Album.vue'
export default {
  components: {
    Layout,
    Album
  },
  setup() {
    const route = useRoute()
    const id = computed(() => {
      return route.params.id
    })
    const store = useStore()
    const albums = computed(() => {
      return store.state.albums.all
    })
    watch(id, (newVal) => {
      store.dispatch('photos/fetchPhotosForAlbum', { id: newVal })
    }, {
      immediate: true
    })
    //I had started to input const photos here but this was a.already established in photoview.vue and b.i had started the renaming process to photosForCurrentAlbum 
    //and got caught on a call with a client
    //I had also previously had 'onMounted' here and deleted it because I am supposed to use 'onBeforeMount' and I'm still not entirely clear about the distinction
    //both are using Composition API lifecycle hooks - do we prefer composition API or options API??
    //before the component is created once the props are resolved??? https://v3.vuejs.org/api/composition-api.html#lifecycle-hooks
    onBeforeMount(() => {
      store.dispatch('albums/fetch')
    })
    return {
      albums,
    }
  }
}
</script>


<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
}
</style>