<template>
  <header class="h-auto flex flex-col justify-center items-center md:py-10">
    <h1 class="text-5xl text-green-500 font-bold text-center my-4">
      Rick & Morty <span class="text-2xl text-black">Personajes</span>
    </h1>
    <button
      v-on:click="fetch"
      class="
        bg-green-400
        w-30
        p-4
        text-white
        rounded-2xl
        font-bold
        hover:bg-green-500
        active:bg-green-800
      "
    >
      Consultar
    </button>
  </header>

  <main class="mx-6 lg:mx-20">
    <ul class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4">
      <Character
        v-for="character of characters"
        v-bind:key="character.id"
        v-bind:character="character"
      />
    </ul>
  </main>
</template>

<script>
import axios from 'axios';

import Character from './components/Character.vue';

export default {
  name: 'App',
  components: {
    Character,
  },
  data() {
    return { URL: 'https://rickandmortyapi.com/api/character', characters: [] };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      let result = axios
        .get(this.URL)
        .then((res) => {
          this.characters = res.data.results;
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style></style>
