<template lang="html">
  <div class="about-container">
    <h1>HOOP TRADE</h1>
    <div class="about-text">
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
  height: 100vh;
  padding: 40px 35px;
  margin-top: 20px;
  display: flex;
  flex-flow: column wrap;
  color: #fff;
}

@media only screen and (max-width: 48em) {
  .about-text {
    margin-top: 10px;
    padding: 20px 10px;
    height: 40vh;
    background: url('http://a.espncdn.com/combiner/i?img=%2Fphoto%2F2016%2F0112%2Fr43546_4031x2867cc.jpg');
    background-size: cover;
    border-radius: 8px;
    filter:drop-shadow(1px 1px 3px black);
  }
}
</style>
