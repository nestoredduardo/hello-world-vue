<template>
  <header class="h-auto flex flex-col justify-center items-center lg:py-10">
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
      <li v-for="character of characters" v-bind:key="character.id">
        <article class="flex flex-col m-1 shadow-md">
          <img v-bind:src="character.image" v-bind:alt="character.name" />
          <div class="w-full m-4">
            <h2 class="font-bold text-xl mb-2">{{ character.name }}</h2>
            <button
              class="
                bg-green-400
                w-30
                p-3
                text-white
                rounded-2xl
                font-bold
                hover:bg-green-500
                active:bg-green-800
              "
            >
              Ver más
            </button>
          </div>
        </article>
      </li>
    </ul>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return { URL: 'https://rickandmortyapi.com/api/character', characters: [] };
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
