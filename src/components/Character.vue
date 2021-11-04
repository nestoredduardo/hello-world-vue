<template>
  <li class="flex flex-col m-3 shadow-md rounded-xl">
    <img
      v-bind:src="character.image"
      v-bind:alt="character.name"
      class="rounded-xl"
    />
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
  </li>
</template>

<script>
export default {
  props: ['character'],
};
</script>
