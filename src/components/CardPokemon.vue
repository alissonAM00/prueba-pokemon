<template>
  <div class="container my-5">
    <div class="card" >
      <img :src="imagenPokemon" class="card-img-top"
        :class="hidePokemon ? 'borroso' : ''" alt="" />
      <div class="card-body">
        <p class="  text-center" v-show="!hidePokemon">{{
          pokemon.name }}</p>
         <form v-show="hidePokemon">
          <input class="form-control" placeholder="Escribe el nombre aquí"
            type="text" v-model="pokemonName" @keyup.enter="find">
          <div class="d-grid my-2">
            <button @click.prevent="find">Descubrir</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CardPokemon',
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
    imagenPokemon() {
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
        this.$emit("respuestaCorrecta")
      } else {
        console.log("incorrecto")
        const name = this.pokemon.name
      
        this.$swal.fire({
          title: 'Ups!',
          text: `Intenta nuevamente. El nombre del pokémon comienza con ${name.substring(0, 3)}`,
          imageUrl: 'https://www.cultture.com/pics/2021/03/pokemon-las-10-mejores-cualidades-del-equipo-rocket-0.jpg',
          background:'rgb(135, 206, 235,0.5)',
          confirmButtonText: 'Continuar',
          

        });
      }
    }
  }
}
</script>


<style scoped>

.borroso {
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

button {
  background: transparent;
  color: #120314;
  font-size: 17px;
  text-transform: uppercase;
  font-weight: 600;
  border: none;
  padding: 20px 30px;
  perspective: 30rem;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.308);
}

button::before {
  content: '';
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  border-radius: 10px;
  background: linear-gradient(320deg, rgba(7, 55, 94, 0.779), rgba(128, 0, 128, 0.308));
  z-index: 1;
  transition: background 3s;
}

button:hover::before {
  animation: rotate 1s;
  transition: all .5s;
}

@keyframes rotate {
  0% {
    transform: rotateY(180deg);
  }

  100% {
    transform: rotateY(360deg);
  }
}

.card{
  background-color: rgba(0, 0, 0, 0.308);
  border-radius: 50px;
  box-shadow: 0 2px 4px rgba(0,0,0, 0.1);
  transition: transform 0.3s ease;
}
.card:hover{
  transform: scale(1.05);
}
</style>
