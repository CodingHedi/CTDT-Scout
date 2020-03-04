<template>
<div>
  <div class="filter">
    <select v-model="selected">
      <option disabled value="">Choisissez</option>
      <option>{{ typeForColor.bleu }}</option>
      <option>{{ typeForColor.rouge }}</option>
      <option>{{ typeForColor.vert }}</option>
    </select>
    &nbsp;&nbsp;&nbsp;<span> Sélectionné : {{ selected }}</span>
    <br />
    <br />
  </div>
  <div class="item-container">
    <div v-for="item in filteredPlayers" v-bind:key="item.id" class="item">
      {{ item.name }}
      <br />
      <img v-bind:src="image(item.desc.img)" />
      <img v-bind:src="imgSrcForColor(item.desc.color)" class="reduce" />
    </div>
  </div>
</div>
</template>

<script>
import json from '../data/fakedata.json'

const images = require.context('../assets/CTDT_mini_card/', false, /\.gif$/);
const types = require.context('../assets/', false, /\.png$/);
const typeForColor = {
  "bleu": "Vit",
  "rouge": "Force",
  "vert": "Tech"
}

export default {
  name: 'Grid',
  components: {},
  computed: {
    filteredPlayers() {
      return this.playerJSON.players.filter(p => this.type(p.desc.color) === this.selected || this.selected === '')
    }
  },
  methods: {
    type(color) {
      color = color.toLowerCase()
      if (color in typeForColor) {
        return typeForColor[color]
      } else {
        alert("C'EST LA MERDE FFS ! " + color)
      }
    },

    imgSrcForColor(color) {
      const type = this.type(color)
      return types(`./${type}.png`)
    },

    image(id) {
      return images(`./${id}.gif`)
    }
  },
  data() {
    return {
      playerJSON: json,
      selected: '',
      typeForColor: typeForColor
    }
  }
}
</script>

<style>
.item-container {
  width: 80%;
  margin-left: auto;
  margin-right: auto;

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.item {
  width: 128px;
  height: 128px;
  margin-bottom: 10px;

  flex-grow: 1 2 3;
}

.reduce {
  width: 18%;
  height: 18%;
}
</style>
