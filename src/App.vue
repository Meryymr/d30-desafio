<template>
  <div id="app" class="background">
    <div class="container">
      <header>
        <div>
          <img class="pokeLogo container mb-3"
            src="./assets/Pokemon_logo.svg.png" alt="">
          <h3 class=" pokeTitle text-center">¿Quién es ese Pokémon?</h3>
          <h5 class="text-center fw-bold">Pokemones descubiertos: <span> {{
            counter }}</span>/32
          </h5>
        </div>
      </header>
      <main>
        <div class="row">
          <div class="col-12 col-md-4 col-lg-3"
            v-for="(pokemon, index) in pokemones" :key="index">
            <CardPoke :pokemon="pokemon" @correctAnswer="increase" />
            <!--a la izquierda está el nombre de Props y la derecha el valor que se le entrega /data binding-->
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CardPoke from './components/CardPoke.vue'

export default {
  name: 'App',
  components: {
    CardPoke
  },
  data() {
    return {
      pokemones: [],
      counter: 0
    }
  },
  methods: {
    async getPoke() {
      try {
        let response = await axios.get("https://pokeapi.co/api/v2/pokemon/?offset=20&limit=32");
        console.log(response)
        this.pokemones = response.data.results
      } catch (error) {
        console.log(error)
      }
    },
    increase() {
      this.counter++
    },
  },
  mounted() {
    this.getPoke()
  }

}
</script>

<style scoped>
* {
  font-family: "Play", sans-serif;
}

.pokeLogo {
  width: 300px;
  display: flex;
  justify-content: center;
}

.pokeTitle {
  font-family: "Press Start 2P", system-ui;
  color: darkblue;
}

span {
  color: rgb(11, 48, 150)
}

.background {
  background-position: center;
  background-attachment: fixed;
  background-image: url("./assets/wallpaper.png");
  background-size: cover;
}
</style>
