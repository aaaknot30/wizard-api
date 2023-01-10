<script setup>

const props = defineProps({
  singleSpell: Object
})

</script>

<template>
  <div class="elixir-details">
  <h2 class="spell-title">{{ singleSpell.name }}</h2>
  <p><strong>Effect: </strong>{{ singleSpell.effect ? singleSpell.effect : "Unknown" }}</p>
  <p><strong>Type: </strong>{{ singleSpell.type ? singleSpell.type : "Unknown"}}</p>
  <p><strong>Light: </strong>{{ singleSpell.light ? singleSpell.light : "Unknown" }}</p>
  <p><strong>Creator: </strong>{{ singleSpell.creator ? singleSpell.creator : "Unknown" }}</p>
  <p><strong>Incantation: </strong>{{ singleSpell.incantation ? "" : "Unknown" }}</p>
</div>
</template>
