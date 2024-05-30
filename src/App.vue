<template>
  <div class="img-fondo portada" id="app">
    <div class="container">
      <header class="text-center pt-4">
        <img src="./assets/img/Pokemon.svg" />
        <h2>¿Qué pokemon se esconde aquí?</h2>
        <h5>
          Pokemones descubiertos: <span> {{ counter }}</span>
        </h5>
      </header>
      <div class="row g-3">
        <main
          class="col-12 col-md-4 col-lg-3"
          v-for="(pokemon, index) in pokemones"
          :key="index"
        >
          <!--  a la izquierda los props del hijo -->
          <CardPokemon :pokemon="pokemon" @respuestaCorrecta="incrementar" />
        </main>
      </div>
      <footer class="container">
        <div class="row">
          <div class="col">
            <div class="pokebola">
              <img src="./assets/img/pokebola.png" alt="" />
            </div>
            <div class="texto">
              <h5 class="d-flex justify-content-center align-items-center fs-3">Alisson 2024</h5>
            </div>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CardPokemon from "./components/CardPokemon.vue";
export default {
  name: "App",
  components: {
    CardPokemon,
  },
  data() {
    return {
      pokemones: [],
      counter: 0,
    };
  },
  methods: {
    async getPoke() {
      try {
        let response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon/?offset=80&limit=32"
        );
        console.log(response);
        this.pokemones = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    incrementar() {
      this.counter++;
    },
  },
  mounted() {
    this.getPoke();
  },
};
</script>

<style>
.img-fondo {
  background-image: url("./assets/img/fondo.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
}
span {
  font-family: "Rusdy Ibra", sans-serif;
}
@keyframes rebotar {
  0% , 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-50px);
  }
  
}
.texto {
  background: linear-gradient(rgba(198, 198, 81, 0.553),rgb(92, 14, 14));
  -webkit-background-clip: text;
  color: transparent;  
  
}


.pokebola {
  animation: rebotar 0.5s infinite;
  position: relative;
  display: inline-block;

}
 .portada{
  font-family: 'Pokemon Solid', sans-serif;
 }
</style>
