<template lang="html">
  <div class="home-container">
    <div :class="{
        'bg': searchName.length < 3,
        'bg-selected': searchName.length >= 3 || matchList.length > 0
      }">
      <div :class="{
          'big-text': searchName.length < 3,
          'big-text-selected': searchName.length >= 3 || matchList.length > 0
        }">
        <p id="title">Need fantasy basketball trade recommendations?</p>
        <p>Pick a player you would like to trade, and we will help you find suggestions</p>
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
    <div class="arrow-container">
      <i class="material-icons" id="arrow-down" v-if="arrow">keyboard_arrow_down</i>
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
    Playercards,
  },
  data () {
    return {
      playersList: [],
      playerNames: [],
      matchList: [],
      searchName: '',
      playerJSON: [],
      player: '',
      arrow: false

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
      this.toggleArrow();
      setTimeout(() => {
        return this.toggleArrow();
      }, 5000);
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
      this.matchList = list.slice(0,11);
      this.searchName = '';
    },
    toggleArrow() {
      this.arrow = !this.arrow;
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

#title {
  color: #fff;
  font-family: sans-serif;
  font-weight: bold;
  font-size: 24px;
  margin: 0;
}

.bg {
  width: 100%;
  height: calc(100vh - 30px);
  display: flex;
  justify-content: center;
}

.bg-selected {
  width: 100%;
  height: 12em;
  display: flex;
  justify-content: center;

  .search {
    top: 8em;
  }
}

.big-text {
  position: absolute;
  top: 37%;
  text-align: center;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
  margin: 0;
  padding: 10px 30px;
  color: #bdc3c7;
}

.big-text-selected {
  display: none;
}
.search {
  position: absolute;
  top: 55%;
  left: 50%;
  width: calc(48em - (48em*0.2));
  overflow: visible;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  margin: auto;
  box-sizing: border-box;
  border: 1px solid $secondary;
  background: rgb(255, 255, 255);
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.search-bar {
  margin: 5px auto;
  width: 90%;
  height: 35px;
  border-style: none;
}

#arrow-down {
  color: #fff;
  padding-bottom: 20px;
  animation: animated-bounce 2s ease;
}

.arrow-container {
  height: 30px;
  padding: 10px 0;
}

@keyframes animated-bounce {
    0%, 20%, 50%, 80%, 100% {transform: translateY(0)}
    40% {transform: translateY(-30px)}
    60% {transform: translateY(-15px)}
}

@media only screen and (max-width: 48em) {
  .big-text {
    top: 35%;
  }
  .search {
    width: calc(100% - 20vw);
  }
  .search-bar {
    padding-left: 20px;
  }
}

@media only screen and (max-height: 28em ) {
  .search {
    top: 16em;
  }

  .big-text {
    top: 10em;
  }
}

@media only screen and (min-width: 1080px) {
  .search {
    top: 50%;
  }
}
</style>
