<template>
  <div class="container my-5">
    <div class="card" style="width: 18rem">
      <img :src="imagePokemon" class="card-img-top"
        :class="hidePokemon ? 'blur' : ''" alt="" />
      <div class="card-body">
        <p class=" paragraphName text-center" v-show="!hidePokemon">{{
          pokemon.name }}</p>
        <form v-show="hidePokemon">
          <input class="form-control" placeholder="Escribe el nombre aquí"
            type="text" v-model="pokemonName" @keyup.enter="find">
          <div class="d-grid my-2">
            <button class="btn" @click.prevent="find">Descubrir</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CardPoke',
  props: {
    pokemon: Object,
  },
  data() {
    return {
      infoPokemon: {},
      hidePokemon: true,
      pokemonName: "",

    }
  },
  computed: {
    imagePokemon() {
      return this.infoPokemon.sprites?.other['official-artwork'].front_default
    }
  },
  created() {
    this.getInfoPokemon()
  },
  methods: {
    async getInfoPokemon() {
      try {
        let { data } = await axios.get(this.pokemon.url)
        this.infoPokemon = data
      } catch (error) {
        console.log(error)
      }
    },
    find() {
      if (this.pokemonName.toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.hidePokemon = false
        this.$emit("correctAnswer")
      } else {
        console.log("incorrecto")
        const name = this.pokemon.name
        this.$swal.fire({
          title: 'Ups!',
          text: `Intenta nuevamente. El nombre del pokémon comienza con ${name.substring(0, 3)}`,
          icon: 'error',
          confirmButtonText: 'Continuar'
        })
      }
    }
  }
}
</script>


<style scoped>
.btn {
  background-color: rgb(37, 83, 249);
  color: rgb(255, 255, 255)
}

.btn:hover {
  box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24), 0 10px 20px 0 rgba(0, 0, 0, 0.19);
}

.blur {
  filter: blur(5px) grayscale(100%);
}

/*Para no poder arrastrar la imagen en la pag*/
img {
  user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

.paragraphName {
  font-family: "Press Start 2P", system-ui;
  text-transform: capitalize;
}

.card {
  background-color: rgba(180, 249, 255, 0.526);
  border-radius: 10%;
}
</style>
