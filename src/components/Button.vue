<template>
<button type="button" v-on:click="$emit('onclick-button', name)">
  Je suis le bouton {{ name }} ({{ last }})
</button>
</template>


<script>
export default {
  name: 'Buttonz',
  props: {
    name: String,
    last: String,
  }
}
</script>
