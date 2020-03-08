<template>
<div>
  <form>
    <fieldset class="fieldset">
      <legend class="legend">
        Liste des joueurs
      </legend>
      <div class="filter">
        <div class="row .align-top">
          <div class="row">
            <input type="checkbox" id="vit" :value="VIT" v-model="checkedTypes" hidden="true">
            <label for="vit"><img v-bind:src="imgSrcForColor(1)" /></label>
            <input type="checkbox" id="force" :value="FORCE" v-model="checkedTypes" hidden="true">
            <label for="force"><img v-bind:src="imgSrcForColor(2)" /></label>
            <input type="checkbox" id="tec" :value="TEC" v-model="checkedTypes" hidden="true">
            <label for="tec"><img v-bind:src="imgSrcForColor(3)" /></label>
          </div>
          <div class="column grow margin-left">
            <div class="autocomplete row">
              <input type="text" v-model="search" @input="onChange" class="grow" />
            </div>
            <ul v-show="isOpen" class="autocomplete-results">
              <li v-for="(item) in filteredPlayerslist" v-bind:key="item.IDFicJoueur" @click="setResult(item.NomJoueur)" class="autocomplete-result">
                {{ item.NomJoueur }}
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div class="item-container">
        <div v-for="item in filteredPlayerslist" v-bind:key="item.IDFicJoueur" class="item">
          <div class="column">
            <div class="row align-center grow">
              <div>
                <img v-bind:src="imgSrcForColor(item.IDFicCouleur)" class="type" />
              </div>
              <div class="name grow">
                {{ item.NomJoueur }}
              </div>
            </div>
            <div class="row align-bottom">
              <img v-bind:src="image(item.IDFicJoueur)" />
            </div>
          </div>
        </div>
      </div>
    </fieldset>
  </form>
</div>
</template>

<script>
import json from '../data/joueurs.json'

const VIT = 'Vit'
const FORCE = 'Force'
const TEC = 'Tec'

const types = require.context('../assets/', false, /\.png$/);
const typeForColor = {
  1: VIT,
  2: FORCE,
  3: TEC
};

export default {
  name: 'Grid',
  components: {},
  computed: {
    filteredPlayerslist() {
      let currentPlayers = this.playerJSON._SOURCE_sddSQL_30;
      currentPlayers = this.filteredPlayersByType(currentPlayers)
      //currentPlayers = this.filteredPlayersByRegion(currentPlayers)
      //currentPlayers = this.filteredPlayersByRarity(currentPlayers)
      currentPlayers = this.filteredPlayersByName(currentPlayers)
      return currentPlayers
    }
  },
  methods: {

    filteredPlayersByName(currentPlayers) {
      return currentPlayers.filter(item => item.NomJoueur.toLowerCase().indexOf(this.search.toLowerCase()) > -1)
    },

    setResult(item) {
      this.search = item;
      this.isOpen = false;
    },

    filteredPlayersByType(currentPlayers) {
      return currentPlayers.filter(p => this.checkedTypes.includes(this.type(p.IDFicCouleur)))

    },

    type(color) {
      if (color in typeForColor) {
        return typeForColor[color]
      } else {
        console.log(`[WARN] : ${Date(Date.now()).toString()} : ${color}.`)
      }
    },

    imgSrcForColor(color) {
      const type = this.type(color)
      if (!type) {
        return types('./gem.png')
      }
      return types(`./${type}.png`)
    },

    image(id) {
      try {
        return require(`../assets/CTDT_mini_card/${id}.gif`)
      } catch (e) {
        return require('../assets/CTDT_mini_card/default.gif')
      }
    },
    onChange() {
      this.isOpen = true;
      if (this.search === '') {
        this.isOpen = false;
      }
    }
  },
  data() {
    return {
      playerJSON: json,
      checkedTypes: [VIT, FORCE, TEC],
      search: '',
      typeForColor: typeForColor,
      isOpen: false
    }
  },
  created() {
    this.VIT = VIT
    this.FORCE = FORCE
    this.TEC = TEC
  }
}
</script>

<style>
.column {
  display: flex;
  flex-direction: column;
}

.row {
  display: flex;
  flex-direction: row;
}

.type {
  width: 21px;
  height: auto;
}

.name {
  margin-left: 5px;
  text-align: center;
}

.item-container {
  width: 80%;
  margin-top: 50px;
  margin-left: auto;
  margin-right: auto;

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.grow {
  flex-grow: 1;
}

.item {
  width: 128px;
  margin-left: 25px;
  margin-bottom: 10px;
  display: flex;
}

.align-center {
  align-items: center;
}

.align-top {
  align-items: start;
}

.fieldset {
  border: 2px solid #439973;
  border-radius: 10px;
}

.legend {

  color: #439973;
  padding-bottom: 5px;
  font-family: 'Patua One';
  margin-left: 10%;
  font-size: 2em;
  font-weight: bold;
  text-align: center;
  padding: 0;
}

.margin-left {
  margin-left: 20px;
}

.autocomplete {
  display: flex;
  height: 30px;
  flex-grow: 1;
  position: relative;
}

.autocomplete-results {
  padding: 0;
  margin: 0;
  height: 200px;
  border: 1px solid #eeeeee;
  overflow: auto;
}

.autocomplete-result {
  list-style: none;
  text-align: left;
  padding: 4px 2px;
  cursor: pointer;
}

.autocomplete-result:hover {
  background-color: #439973;
  color: #ffffff;
}

input[type="checkbox"]:not(:checked)+label img {
  opacity: 0.5;
  filter: grayscale(1);
}
</style>
