<template>
        <img :style="desenfocado" :src="img" alt="" />
      <!-- filter: blur(5px) grayscale(100%) -->
      <!-- ### De este componente quité todos los [i], te daba error porque era una variable que no estaba definida. Y por otro lado, recuerda que a este componente le estás pasando solo un pokemon para mostrar (viene del v-for en App.vue, es una iteración del array de pokemones que hay allá) -->
        <input v-show="!show"
          v-model="nombreLocal"
          type="search"
          class="form-control"
          @keypress.enter="$emit('descubrirPokemon', i, nombreLocal)"
          style="background-color: lightgoldenrodyellow; margin-bottom: 10px;"
        />
        <p v-show="show" v-if="nombreLocal == nombres">{{ nombres }}</p>
        <!-- ### Al hacer click en el boton, se manda el emit, se envían como argumentos el indice (que llegó como prop) y el nombreLocal (proveniente del input) -->
        <button @click="$emit('descubrirPokemon', i, nombreLocal)" v-show="!show" type="submit" class="btn btn-primary" style="margin-bottom: 10px;" >Descubrir</button>
</template>

<script>
export default {
  name: "PokemonComponent",
  props:{
    // ### Le cambie el tipo a desenfocado, por lo mismo que te explicaba en App.vue
    desenfocado: Object,
    img: String,
    show: Boolean,
    nombre: String,
    nombres: String,
    i: Number
  },
  // ### En vue 3 es necesario registrar los Emits, parecido a como se hace con los props
  emits: ['descubrirPokemon'],
  data: function () {
    return {
      nombreLocal: this.nombre,
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
