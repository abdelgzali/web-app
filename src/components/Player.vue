<template lang="html">
  <div class="parent-container">
    <h3>{{player}}</h3>
    <h4>2018 Season stats (per game):</h4>
    <p>FG: {{fg}}%</p>
    <p>FT: {{ft}}%</p>
    <p>PPG: {{points}}</p>
    <p>Assists: {{assists}}</p>
    <p>Rebounds: {{rebounds}}</p>
    <p>Steals: {{steals}}</p>
    <p>Blocks: {{blocks}}</p>
    <p>Turnovers: {{turnovers}}</p>
  </div>
</template>

<script>

export default {
  data () {
    return {
      player: 'Alex Abrines',
      fg: '',
      ft: '',
      threes: '',
      points: '',
      assists: '',
      rebounds: '',
      steals: '',
      blocks: '',
      turnovers: '',

    }
  },
  created: function() {
    fetch('http://data.nba.net/10s/prod/v1/2018/players/203518_profile.json')
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        this.fg = data.league.standard.stats.latest.fgp;
        this.ft = data.league.standard.stats.latest.ftp;
        this.points = data.league.standard.stats.latest.ppg;
        this.assists = data.league.standard.stats.latest.apg;
        this.rebounds = data.league.standard.stats.latest.rpg;
        this.steals = data.league.standard.stats.latest.spg;
        this.blocks = data.league.standard.stats.latest.bpg;
        this.turnovers = data.league.standard.stats.latest.t0pg;
      })
      .catch((err) => {
        console.log(err);
      })
  }
}
</script>

<style lang="scss">
$primary: #F21A13;
$secondary: #1D428A;

.parent-container {
  width: 300px;
  margin: 20px auto;
  background: #ecf0f1;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  padding: 20px 40px;
}

.parent-container h4 {
  color: $secondary;
}

.parent-container h3 {
  color: $primary;
}
</style>
