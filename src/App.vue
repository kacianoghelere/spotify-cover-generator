<template>
  <div class="container">
    <PlaylistCover />
  </div>
</template>

<script>
  import PlaylistCover from './components/PlaylistCover.vue'

  export default {
    name: 'app',
    components: {
      PlaylistCover
    }
  }
</script>

<style lang="scss" scoped>
  .container {
    align-items: center;
    display: flex;
    justify-content: center;
  }
</style>
