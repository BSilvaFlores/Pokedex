<template>
  <header class="container">
    <img width="500"
      src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1280px-International_Pok%C3%A9mon_logo.svg.png"
      alt="pokemonLogo" />

    <h2>¿Quién es ese Pokémon?</h2>

    <p>Pokemones descubiertos: <span style="color: coral; font-weight: bold;">{{ contadorPokemon.length }}</span></p>
  </header>
  <div class="container row mx-auto">
    <div class="col-3" v-for="(data, i) in pokemones" :key="i">
      <!-- ### A este componente le agregue otra Prop. Le paso el i (indice) para que pueda sea usado al hacer el 'descubrirPokemon'  -->
      <PokemonComponent :desenfocado="data.desenfocado" :img="data.img" :show="data.show" :nombre="nombre[i]"
        :nombres="nombres[i]" :i="i" @descubrirPokemon="descubrir" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonComponent from "./components/PokemonComponent.vue";


export default {
  name: "App",
  components: {
    PokemonComponent,
  },

  data() {
    return {
      pokemones: [],
      nombre: [],
      nombres: [],
      contadorPokemon: [],
    };
  },

  methods: {
    // #### Recibe index y name del emit de PokemonComponent
    descubrir(index, name) {
      if (this.nombres[index] == name) {
        // #### Acá también cambié el tipo en desenfocado. En vez de asignarle un string vacío, le asigno un objeto vacío. Porque esa prop esperará un objeto.
        this.pokemones[index].desenfocado = {};
        this.pokemones[index].show = true;
        this.contadorPokemon.push(this.nombre);
      } else {
        alert("Nombre incorrecto");
      }
      if (this.contadorPokemon.length === 20) {
        alert("¡FELICIDADES, HAS COMPLETADO LA POKEDEX DE 20 POKEMONES!");
      }
    },
  },

  mounted() {
    for (let i = 1; i <= 20; i++) {
      let n = Math.floor(Math.random() * 1000);
      if(n === n){
        n++;
      }
      
      axios
        .get(`https://pokeapi.co/api/v2/pokemon/${n}`)
        .then((response) => {
          // #### Cambié el contenido de desenfocado, lo puse dentro de un objeto, recuerda que :style recibe un objeto.
          let desenfocado = { filter: 'blur(5px) grayscale(100%)' };
          let show = false;
          let pokemon = {
            name: response.data.name,
            img: response.data.sprites.front_default,
            id: response.data.id,
            desenfocado: desenfocado,
            show: show,
          };
          this.pokemones.push(pokemon);
          this.nombres.push(pokemon.name);

        })
        .catch((err) => {
          console.log(err);
        });

    }
  },

};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-bottom: 60px;
}
</style>
