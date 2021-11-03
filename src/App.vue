<template>
  <h1 class="">Consultar Datos</h1>
  <button v-on:click="fetch">Consultar</button>

  <div v-for="character of characters" v-bind:key="character.id">
    {{ character }}
  </div>
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
