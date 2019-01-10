<template lang="html">
  <router-link :to="{ name: 'home'}">
    <i id="menu" class="material-icons">menu</i>
  </router-link>
</template>

<script>
export default {
}
</script>

<style lang="css">

@media only screen and (max-width: 600px) {
  #menu {
    position: absolute;
    top: 30px;
    left: 30px;
    color: #fff;
  }
}

</style>
