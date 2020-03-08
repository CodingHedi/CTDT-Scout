<template>
<div>
  <form>
    <fieldset class="fieldset">
      <legend class="legend">
        Liste des joueurs
      </legend>
      <div class="filter">
        <select v-model="selected">
          <option disabled value="">Choisissez</option>
          <option>{{ typeForColor.bleu }}</option>
          <option>{{ typeForColor.rouge }}</option>
          <option>{{ typeForColor.vert }}</option>
        </select>
        <span> Sélectionné : {{ selected }}</span>
        <br />
        <div class="autocomplete">
          <input type="text" v-model="search" @input="onChange" />
          <ul v-show="isOpen" class="autocomplete-results">
            <li v-for="(item) in filteredPlayerslist" v-bind:key="item.IDFicJoueur" @click="setResult(item.NomJoueur)" class="autocomplete-result">
              {{ item.NomJoueur }}
            </li>
          </ul>
        </div>
        <br />
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

const types = require.context('../assets/', false, /\.png$/);
const typeForColor = {
  1: "Vit",
  2: "Force",
  3: "Tech"
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
      return currentPlayers.filter(p => this.type(p.IDFicCouleur) === this.selected || this.selected === '')

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
      selected: '',
      search: '',
      typeForColor: typeForColor,
      isOpen: false
    }
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

.fieldset {
  border: 5px solid #4AAE9B;
  border-radius: 8px;
  box-shadow: 0 0 5px 2px rgba(0, 0, 0, .35);
}

.legend {
  color: #fff;
  background-color: #4AAE9B;
  box-shadow: 0 0 5px 2px rgba(0, 0, 0, .35);
  margin-left: 10%;
  font-size: 2em;
  font-weight: bold;
  text-align: center;
  padding: 0;
}

.autocomplete {
  position: relative;
  width: 130px;
}

.autocomplete-results {
  padding: 0;
  margin: 0;
  border: 1px solid #eeeeee;
  height: 120px;
  overflow: auto;
}

.autocomplete-result {
  list-style: none;
  text-align: left;
  padding: 4px 2px;
  cursor: pointer;
}

.autocomplete-result:hover {
  background-color: #4AAE9B;
  color: white;
}
</style>
