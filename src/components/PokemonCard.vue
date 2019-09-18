<template>
  <div class="pokemon-card">
   <img :src="imgUrl">
   <p>{{ name | capitalize }}</p>
   <ul>
     <li>Weight: {{ weight }}</li>
     <li>Height: {{ height }}</li>
     <li>Types: <span v-for="(type, i) in types" :key="type.type.name">{{ type.type.name | capitalize }}<span v-if="i == 0">, </span></span></li>
   </ul>
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
</style>
