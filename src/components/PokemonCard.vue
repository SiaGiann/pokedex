<template>
  <div class="pokemon-card">

   <v-card
    max-width="344"
    class="justify-center d-flex"
  >
    <v-card-title>{{ name | capitalize }}</v-card-title>
    <v-img
      height="200px"
      width="200px"
      :src="imgUrl"
    />
    <v-card-text>
      <ul>
        <li>Weight: {{ weight }}</li>
        <li>Height: {{ height }}</li>
        <li>Types: <span v-for="(type, i) in types" :key="type.type.name">{{ type.type.name | capitalize }}<span v-if="i !== types.length-1">, </span></span></li>
      </ul>
    </v-card-text>
  </v-card>
  </div>
</template>

<script>
export default {
  props: ["pokemonNumber"],

  data () {
    return {
      name: '',
      imgUrl: '',
      weight: 0,
      height: 0,
      types: []
    }
  },

  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },

  mounted () {
    fetch('https://pokeapi.co/api/v2/pokemon/' +this.pokemonNumber)
      .then(response => response.json())
      .catch(error => console.error('Error:', error))
      .then(res => {
        this.name = res.name;
        this.imgUrl = res.sprites.front_default;
        this.weight = res.weight;
        this.height = res.height;
        this.types = res.types;
      });
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.pokemon-card {
  margin-left: 1rem;
  margin-top: 1rem;
}
</style>
