<script setup>

const props = defineProps({
  singleElixir: Object
})

</script>

<template>
  <div class="elixir-details">
  <h2 class="elixir-title">{{ singleElixir.name }}</h2>
  <p><strong>Effect: </strong>{{ singleElixir.effect ? singleElixir.effect : "Unknown" }}</p>
  <p><strong>Characteristics: </strong>{{ singleElixir.characteristics ? singleElixir.characteristics : "Unknown" }}</p>
  <p><strong>Side Effects: </strong>{{ singleElixir.sideEffects ? singleElixir.sideEffects : "Unknown" }}</p>
  <p><strong>Difficulty: </strong>{{ singleElixir.difficulty ? singleElixir.difficulty : "Unknown"}}</p>
  <p><strong>Ingredients: </strong>{{ singleElixir.ingredients.length > 0 ? "" : "Unknown" }}</p>
  <ul v-for="item in singleElixir.ingredients" :key="item.name">
    <li>{{ item.name }}</li>
  </ul>
</div>
</template>
