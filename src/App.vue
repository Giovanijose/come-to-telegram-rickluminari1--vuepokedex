<script setup lang="ts">
import axios from 'axios';
import { onMounted, ref } from 'vue';

interface Pokemon {
  name: string;
  url: string;
}

const pokemons = ref<Pokemon[]>([]);
const url = "https://pokeapi.co/api/v2/";

onMounted(() => {
  getPokemons();
})

async function getPokemons() {
  try {
    const res = await axios.get(`${url}/pokemon?limit=151&offset=0`);
    pokemons.value = res.data.results;
    // console.log(res.data.results)
  } catch (error) {
    console.error(error);
  }

}

function getImage(pokemonUrl: string): string {
  // Extraí o ID da URL do Pokémon
  const match = pokemonUrl.match(/\/pokemon\/(\d+)\//);
  const id = match ? match[1] : '0';
  return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
}

</script>

<template>

  <div class="box">

    <div v-for="pokemon in pokemons">

      <div class="title">
        <span>{{ pokemon.name }}</span>
      </div>
      <div class="image">
        <img :src="getImage(pokemon.url)" alt="">
      </div>

    </div>

  </div>

</template>

<style scoped lang="scss">
.box {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;

  &>div {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 2px solid #002EA1;
    border-radius: 5px;
    box-shadow: 5px 5px 5px 0px rgba(0, 0, 0, 0.75);
    height: 200px;

    .title {
      width: 100%;
      background-color: #FFCB05;
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
      text-align: center;
      color: #002EA1;
      font-weight: bold;
      border-bottom: 1px solid #002EA1;

      span {
        font-size: 20px;
        text-transform: capitalize;
      }
    }

    .image {
      width: 100%;
      border-bottom-left-radius: 5px;
      border-bottom-right-radius: 5px;
      background: radial-gradient(circle, #8DCBE3 0%,
          #3C5AA6 90%);
      text-align: center;

      &:hover{
        cursor: pointer;
        background: radial-gradient(circle, #8DCBE3 0%,
          #3C5AA6 100%);

          img{
            transform: scale(1.2);
          }
      }

      img {
        width: 173px;
        height: 173px;
      }

    }


  }
}
</style>
