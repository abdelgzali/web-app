<template lang="html">
  <div class="about-container">
    <div class="nav-bar">
      <Nav/>
    </div>
    <div class="about-text">
      <div class="text-container">
        <h3>A valuable tool for every fantasy basketball enthusiast,
          our recommendation engine provides quality trade suggestions for active NBA players
        </h3>
        <p id="fb-ninja">*This tool is powered by fb-ninja's statistics and player rankings.</p>
      </div>
    </div>
  </div>
</template>

<script>
import Nav from '@/components/Nav'

export default {
  name: 'about',
  components: {
    Nav,
  }
}
</script>

<style lang="scss">
$primary: #F21A13;
$secondary: #1D428A;

.about-container {
  width: calc(100% - 4.8em);
  height: calc(100vh - 80px);
  padding: 40px 2.4em;
  display: flex;
  flex-flow: column wrap;
  justify-content: center;
  color: #fff;
  background: url('http://a.espncdn.com/combiner/i?img=%2Fphoto%2F2016%2F0112%2Fr43546_4031x2867cc.jpg');
  background-size: cover;
  background-position: center;
}

.nav-bar {
  position: absolute;
  width: 100%;
  top: 0;
  left: 0;
}

.about-text {
  margin-top: 5em;
  border-radius: 8px;
  filter:drop-shadow(1px 1px 3px black);
}

.text-container {
  display: flex;
  flex-flow: column wrap;
  justify-content: center;
  background: rgba(0, 0, 0, 0.45);
  padding: 1.2em;
  border-radius: 8px;
  width: calc(100% - 2.4em);
  height: calc(100% - 2.4em);
}

#fb-ninja {
  color: $primary;
  font-family: Montserrat, sans-serif;
}

@media only screen and (min-width: 48em) {
  .about-text {
    width: 42em;
    margin: 5vh auto;
  }
}
</style>
