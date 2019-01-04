<template>
  <div class="home">
    <Player/>
  </div>
</template>

<script>
// @ is an alias to /src
import Player from '@/components/Player.vue'

export default {
  name: 'home',
  components: {
    Player,
  }
}
</script>

<style lang="scss">
</style>
