<template lang="html">
  <div class="nav-container">
    <router-link :to="{ name: 'home' }">
      <img id="logo" alt="Vue logo" src="logo2.png">
    </router-link>
    <div class="desktop-view">
      <div class="top-links" v-for="item in barItems">
        <router-link class="router-style" :to="item.link">
          <i class="material-icons">{{item.icon}}</i>
          <div class="link-title">{{item.title.toUpperCase()}}</div>
        </router-link>
      </div>
    </div>
    <div class="mobile-view">
      <i id="hamburger" class="material-icons" @click="toggleSide">menu</i>
      <div class="side-bar" v-if="sideBar">
        <div id="close-btn" @click="toggleSide">
          <i class="material-icons">close</i>
        </div>
        <div class="side-link" v-for="(item, index) in barItems">
          <router-link :to="item.link">
            <i class="material-icons">{{item.icon}}</i>
            <div class="link-title" @click="toggleSide">{{item.title.toUpperCase()}}</div>
          </router-link>
        </div>
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
          icon: 'assignment_ind',
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

<style lang="scss">

@mixin no-select {
  /* prevents accidental text highlighting */
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                        supported by Chrome and Opera */
}

@mixin flex-row {
  display: flex;
  flex-flow: row nowrap;
}

a {
  text-decoration: none;
  color: inherit;
}

.nav-container {
  @include flex-row;
  justify-content: space-between;
  align-items: center;
  @include no-select;
}

#logo {
  height: 75px;
  width: auto;
  padding: 1em;
}

/* DESKTOP LINKS */
.desktop-view {
  right: 0;
  top: 0;
  padding-right: 1em;
  @include flex-row;
  justify-content: flex-end;
}

.top-links {
  color: #fff;
  padding: 2em 1em;
}

.router-style {
  @include flex-row;
  align-items: center;
}

/* MOBILE LINKS */
.side-bar {
  position: absolute;
  z-index: 1;
  left: 0;
  top: 0;
  background-color: #ecf0f1;
  color: #2c3e50;
  height: 100vh;
  width: 40%;
  padding: 20px 20px;
  display: flex;
  flex-flow: column wrap;
  animation: 0.5s ease-out 0s 1 slideInFromLeft;
}

.side-bar:active {
  transition: all .3s linear;
}

.side-link {
  padding: 25px 15px;
}

.side-link a {
  @include flex-row;
  align-items: center;
  justify-content: center;
}

.link-title {
  font-size: 1em;
  margin: 0 10px;
}

#close-btn {
  cursor: pointer;
  display: flex;
  justify-content: flex-end;
}

@media only screen and (max-width: 48em) {
  #logo {
    position: absolute;
    left: auto;
    right: 20px;
    top: 20px;
    height: 50px;
    padding: 0;
  }

  #hamburger {
    position: absolute;
    @include no-select;
    top: 1.2em;
    left: 1.2em;
    color: #fff;
    cursor: pointer;
  }
  .top-links {
    display: none;
  }
}

@media only screen and (min-width: 48em) {
  .mobile-view {
    display: none;
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
