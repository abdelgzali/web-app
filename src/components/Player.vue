<template lang="html">
  <div class="parent-container">
    <input class="search-bar" placeholder="search player" v-model="searchName" @input="findPlayer">
    <div class="player-card">
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
    <table id="table">

    </table>
  </div>
</template>

<script>

export default {
  data () {
    return {
      playersList: [],
      searchName: '',
      player: '',
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
    // full list of NBA players
    fetch('https://data.nba.net/10s/prod/v1/2018/players.json')
      .then((res) => res.json())
      .then((data) => {
        this.playersList = data.league.standard;
        //console.log(this.playersList);
      })
      .catch((err) => {
        console.log(err);
      });

    // NBA PLAYER STATS & COMPUTED Z-SCORES (WEIGHTED) via FB-Ninja
    // fetch JSON from file
    fetch("players.xlsx")
      .then((res) => {
        // get data as blob
        if(!res.ok) throw new Error ("Players data fetch failed");
        return res.arrayBuffer();
      })
      .then ((ab) => {
        // parse data
        const data = new Uint8Array(ab);
        const XLSX = require('xlsx');
        const wb = XLSX.read(data, {
          type:"array"
        });
        // store data in JSON array
        const sheet = wb.SheetNames[0];
        const playerSheet = wb.Sheets[sheet];
        const playerJSON = XLSX.utils.sheet_to_json(playerSheet);
        console.log(playerJSON);
      })
  },
  methods: {
    findPlayer() {
      const list = this.playersList;
      list.forEach((player) => {
        let foundName = player.firstName + " " + player.lastName;
        // fuzzy search: foundName.toLowerCase().indexOf(this.searchName.toLowerCase()) !== -1
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.getStats(player.personId);

        }
      })
    },
    getStats(id) {
      console.log("looking up player");
      let uri = "https://data.nba.net/10s/prod/v1/2018/players/" + id +"_profile.json";
      fetch(uri)
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
          this.turnovers = data.league.standard.stats.latest.topg;
        })
        .catch((err) => {
          console.log(err);
        })
    }
  }
}
</script>

<style lang="scss">
$primary: #F21A13;
$secondary: #1D428A;

*:focus {
    outline: none;
}

.parent-container {
}

.search-bar {
  padding: 5px 40px;
  margin: 20px auto;
  width: 300px;
  height: 35px;
  border: 1px solid $secondary;
  border-radius: 40px;
}


.player-card {
  background: #ecf0f1;
  padding: 20px 40px;
  margin: 20px auto;
  width: 300px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}
.parent-container h4 {
  color: $secondary;
}

.parent-container h3 {
  color: $primary;
}
</style>
