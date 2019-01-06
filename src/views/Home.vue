<template lang="html">
  <div class="parent-container">
    <div class="blk-bg">
      <div class="big-text">
        <p id="title">Need fantasy basketball trade recommendations?</p>
        <p>Don't worry, we won't suggest Ricky Rubio</p>
      </div>
      <div class="search">
        <input
          class="search-bar"
          placeholder="search active NBA players"
          v-model="searchName"
          @keyup="searchName = $event.target.value; findPlayer"
          type="search">
        <Autocomplete
          :items="playerNames"
          v-bind:tQuery="searchName"
          @selected="playerSelected"/>
      </div>
    </div>
    <div class="suggested-players">
      <Playercards :players="matchList" v-bind:name="searchName"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Autocomplete from '@/components/Autocomplete';
import Playercards from '@/components/Playercards';

export default {
  name: 'home',
  components: {
    Autocomplete,
    Playercards
  },
  data () {
    return {
      playersList: [],
      playerNames: [],
      matchList: [],
      searchName: '',
      playerJSON: [],
      player: '',

    }
  },
  created: function() {
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
        this.playersList = XLSX.utils.sheet_to_json(playerSheet);
        this.indexNames();
      });
  },
  methods: {
    indexNames() {
      const list = this.playersList;
      list.forEach((player) => {
        let name = player.NAME;
        this.playerNames.push(name);
      });
    },

    playerSelected(player) {
      this.searchName = player;
      this.findPlayer();
    },

    findPlayer() {
      let index = 0;
      this.playersList.forEach((player) => {
        let foundName = player.NAME;
        if (foundName.toLowerCase() == this.searchName.toLowerCase()) {
          console.log(foundName + " found");
          this.player = foundName;
          this.findMatch(index);
        };
        index++;
      });
    },

    findMatch(index) {
      let list = this.playersList;
      let searchedPlayer = this.playersList[index];
      list.sort((firstPlayer, secondPlayer) => {
        let firstVal = Math.abs(firstPlayer.VALUE - searchedPlayer.VALUE);
        let secondVal = Math.abs(secondPlayer.VALUE - searchedPlayer.VALUE);
        if (firstVal > secondVal) {
          return 1;
        } else if (firstVal < secondVal) {
          return -1
        } else {
          return 0;
        }
      })
      //console.log(list[0].NAME + " matches logged");
      this.matchList = list.slice(0,11);
      //console.log(this.matchList);
      this.searchName = '';
    }
  }
}
</script>

<style lang="scss">

$primary: #F21A13;
$secondary: #1D428A;

@mixin flex-col {
  display: flex;
  flex-flow: column wrap;
  align-items: stretch;
}

*:focus {
    outline: none;
}

#title {
  color: #fff;
  font-family: sans-serif;
  font-weight: bold;
  font-size: 24px;
  margin: 0;
}

.blk-bg {
  background: #000;
  width: 100%;
  padding: 10px 0;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12);
}

.big-text {
  @include flex-col;
  justify-content: flex-end;
  margin: 0;
  padding: 10px 40px;
  color: #bdc3c7;
}
.search {
  width: 380px;
  margin: 20px auto;
  border: 1px solid $secondary;
  border-radius: 40px;
  transition: all .5s linear;
  background: rgb(255, 255, 255);
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.search:hover {
  border-radius: 5px;
  transition: all .5s linear;
}

.search-bar {
  margin: 5px auto;
  width: 90%;
  height: 35px;
  border-style: none;
}

@media only screen and (max-width: 420px) {
  .search {
    width: 300px;
  }

  .search-bar {
    padding-left: 20px;
    border-radius: 40px;
  }
}
</style>
