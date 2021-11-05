<template>
  <div
    class="fixed w-full h-full z-10 bg-black bg-opacity-75"
    v-if="modal"
  ></div>

  <section
    class="
      fixed
      z-20
      bg-white
      modal
      p-8
      rounded-xl
      w-1/2
      flex flex-col
      items-center
    "
    v-if="modal"
  >
    <h1 class="text-5xl font-bold text-green-500">
      {{ currentCharacter.name }}
    </h1>
    <img
      v-bind:src="currentCharacter.image"
      v-bind:alt="currentCharacter.name"
      class="rounded-full w-60 h-60 my-4"
    />
    <h2 class="font-semibold text-3xl">Género</h2>
    <p class="font-medium text-2xl text-blue-500 mb-3">
      {{ currentCharacter.gender }}
    </p>
    <h2 class="font-semibold text-3xl">Estatus</h2>
    <p class="font-medium text-2xl text-blue-500 mb-3">
      {{ currentCharacter.status }}
    </p>
    <h2 class="font-semibold text-3xl">Especie</h2>
    <p class="font-medium text-2xl text-blue-500 mb-3">
      {{ currentCharacter.species }}
    </p>
    <h2 class="font-semibold text-3xl">Tipo</h2>
    <p class="font-medium text-2xl text-blue-500 mb-3">
      {{ currentCharacter.type || 'No hay datos' }}
    </p>
    <button
      @click="() => (modal = false)"
      class="
        border border-blue-500
        rounded
        py-1
        px-3
        bg-blue-500
        text-white text-2xl
      "
    >
      Cerrar
    </button>
  </section>

  <header class="h-auto flex flex-col justify-center items-center md:py-10">
    <h1 class="text-5xl text-green-500 font-bold text-center my-4">
      Rick & Morty <span class="text-2xl text-black">Personajes</span>
    </h1>

    <form
      action="submit"
      v-on:submit.prevent="searchData"
      v-on:keyup.enter="searchData"
      class="flex items-center w-full justify-center"
    >
      <input
        type="text"
        v-model="search"
        class="
          w-5/12
          text-sm
          p-3
          mr-2
          border-2 border-blue-500
          rounded-md
          focus:outline-none focus:bg-blue-300
        "
      />

      <button
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
        Buscar
      </button>
    </form>
  </header>

  <nav>
    <ul class="flex my-4 justify-center">
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-white
            rounded
            hover:border-gray-200
            text-blue-500
            hover:bg-gray-200
            py-1
            px-3
          "
          href="#"
          v-on:click="changePage(page - 1)"
          >Anterior</a
        >
      </li>
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-blue-500
            rounded
            py-1
            px-3
            bg-blue-500
            text-white
          "
          href="#"
          >{{ page }}</a
        >
      </li>
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-white
            rounded
            hover:border-gray-200
            text-blue-500
            hover:bg-gray-200
            py-1
            px-3
          "
          href="#"
          v-on:click="changePage(page + 1)"
          >Siguiente</a
        >
      </li>
    </ul>
  </nav>

  <main class="mx-6 lg:mx-20">
    <ul class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4">
      <Character
        @showModal="showModal"
        v-for="character of characters"
        v-bind:key="character.id"
        v-bind:character="character"
      />
    </ul>
  </main>

  <nav>
    <ul class="flex my-4 justify-center">
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-white
            rounded
            hover:border-gray-200
            text-blue-500
            hover:bg-gray-200
            py-1
            px-3
          "
          href="#"
          v-on:click="changePage(page - 1)"
          >Anterior</a
        >
      </li>
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-blue-500
            rounded
            py-1
            px-3
            bg-blue-500
            text-white
          "
          href="#"
          >{{ page }}</a
        >
      </li>
      <li class="mr-3">
        <a
          class="
            inline-block
            border border-white
            rounded
            hover:border-gray-200
            text-blue-500
            hover:bg-gray-200
            py-1
            px-3
          "
          href="#"
          v-on:click="changePage(page + 1)"
          >Siguiente</a
        >
      </li>
    </ul>
  </nav>
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
    return {
      URL: 'https://rickandmortyapi.com/api/character',
      characters: [],
      page: 1,
      pages: 1,
      search: '',
      modal: false,
      currentCharacter: [],
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search,
      };

      let result = axios
        .get(this.URL, { params })
        .then((res) => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      console.log(this.search);
      this.page = 1;
      this.fetch();
    },
    showModal(id) {
      this.fetchOne(id);
    },
    async fetchOne(id) {
      const result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}`
      );

      this.currentCharacter = result.data;
      this.modal = true;

      console.log(this.currentCharacter);
    },
  },
};
</script>

<style>
.modal {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
