<template lang="html">
  <div class="nav-container">
    <i id="hamburger" class="material-icons" @click="toggleSide">menu</i>
    <div class="side-bar" v-if="sideBar">
      <div id="close-btn" @click="toggleSide">
        <i class="material-icons">close</i>
      </div>
      <div class="side-link" v-for="(item, index) in barItems">
        <router-link :to="item.link">
          <span @click="toggleSide">{{item.title}}</span>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sideBar: false,
      barItems: [
        {
          title: 'Home',
          icon: 'home',
          link: '/'
        },
        {
          title: 'About',
          icon: 'about',
          link: '/about'
        }
      ]
    }
  },
  methods: {
    toggleSide() {
      this.sideBar = !this.sideBar;
    }
  }
}
</script>

<style lang="css">

.nav-container {
  /* prevents accidental text highlighting */
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                        supported by Chrome and Opera */
}

.side-bar {
  position: absolute;
  z-index: 1;
  left: 0;
  top: 0;
  background-color: #ecf0f1;
  height: 100vh;
  width: 40%;
  padding: 50px 10px;
  display: flex;
  flex-flow: column wrap;
  animation: 0.5s ease-out 0s 1 slideInFromLeft;
}

.side-bar:active {
  transition: all .3s linear;
}

.side-link {
  padding: 15px 5px;
}

.side-link a {
  text-decoration: none;
  color: inherit;
}

#close-btn {
  cursor: pointer;
  position: absolute;
  top: 20px;
  left: 80%;

}

@media only screen and (max-width: 48em) {
  #hamburger {
    position: absolute;
    top: 30px;
    left: 30px;
    color: #fff;
    cursor: pointer;
  }
}

@keyframes slideInFromLeft {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(0);
  }
}

</style>
