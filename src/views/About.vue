<template lang="html">
  <div class="about-container">
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

export default {
  name: 'about',
  components: {
  }
}
</script>

<style lang="scss">
$primary: #F21A13;
$secondary: #1D428A;

.about-container {
  width: calc(100% - 70px);
  height: calc(100vh - 80px);
  padding: 40px 35px;
  display: flex;
  flex-flow: column wrap;
  justify-content: center;
  color: #fff;
  background: url('http://a.espncdn.com/combiner/i?img=%2Fphoto%2F2016%2F0112%2Fr43546_4031x2867cc.jpg');
  background-size: cover;
}

.about-text {
  height: 50vh;
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
}

@media only screen and (min-width: 48em) {
  .about-container {
    height: calc(100vh - 12em);
  }
  .about-text {
    width: 42em;
    margin: 5vh auto;
  }
}
</style>
