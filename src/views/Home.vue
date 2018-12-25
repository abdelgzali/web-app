<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Player/>
  </div>
</template>

<script>
// @ is an alias to /src
import Player from '@/components/Player.vue'

export default {
  name: 'home',
  components: {
    Player
  }
}
</script>

<style lang="scss">

.home img {
  width: 40%;
  height: auto;
}
</style>
