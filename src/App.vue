<script setup>
import { reactive, ref, watch, watchEffect } from 'vue'
import Elixir from './components/Elixir.vue'
import Spell from './components/Spell.vue'
import Ingredient from './components/Ingredient.vue'

// ----------------------------------------------------------------

// Elixirs
const elixirs = ref(null)
const ELIXIRS_URL = `https://wizard-world-api.herokuapp.com/Elixirs`
watchEffect(async () => {
  const data = await (await fetch(ELIXIRS_URL)).json()
  elixirs.value = data
  //console.log(elixirs)
})

// singleElixir
const selected = ref('Fergus Fungal Budge')
let singleElixir = reactive({
  name: "Fergus Fungal Budge",
  id: "0106fb32-b00d-4d70-9841-4b7c2d2cca71",
  effect: "Treats ringworm, fungicide",
  sideEffects: "Potential negative side effects if used by elves",
  characteristics: "",
  ingredients: [{ name: "Neem oil" }, { name: "Jewelweed" }, { name: "Onion juice" }]
})

// ShowElixir 
const showElixir = () => {
  elixirs.value.filter(item => {
    if (item.name === selected.value) {
      singleElixir.name = item.name
      singleElixir.id = item.id
      singleElixir.effect = item.effect
      singleElixir.sideEffects = item.sideEffects
      singleElixir.characteristics = item.characteristics
      singleElixir.ingredients = item.ingredients

    }
  })
}

// -------------------------------------------------------------

// Spells
const spells = ref(null)
const SPELLS_URL = `https://wizard-world-api.herokuapp.com/Spells`
watchEffect(async () => {
  const data = await (await fetch(SPELLS_URL)).json()
  spells.value = data
  //console.log(spells)
})

// singleSpell
const selectedSpell = ref('Opening Charm')
let singleSpell = reactive({
  name: "Opening Charm",
  incantation: "Aberto",
  effect: "Opens doors",
  type: "Charm",
  light: "Blue",
  creator: null
})

// ShowSpell
const showSpell = () => {
  spells.value.filter(item => {
    if (item.name === selectedSpell.value) {
      singleSpell.name = item.name
      singleSpell.incantation = item.incantation
      singleSpell.effect = item.effect
      singleSpell.type = item.type
      singleSpell.light = item.light
      singleSpell.creator = item.creator

    }
  })
}

// -------------------------------------------------------------

// Ingredients
const ingredients = ref(null)
const INGREDIENTS_URL = `https://wizard-world-api.herokuapp.com/Ingredients`
watchEffect(async () => {
  const data = await (await fetch(INGREDIENTS_URL)).json()
  ingredients.value = data
  //console.log(ingredients)
})

// singleIngredient
let selectedIngredient = ref('Newt spleens')
let singleIngredient = reactive({
  name: "Newt spleens"
})

// Create Elixir 
const createElixirIngredients = ref([])
const createElixirMessage = ref({})

// ShowIngredients
const showIngredients = () => {
  ingredients.value.filter(item => {
    if (item.name === selectedIngredient.value) {
      singleIngredient.name = item.name
      createElixirIngredients.value.push(singleIngredient.name)
    }
  })
}

function createElixir() {
  const rnd = Math.ceil(Math.random() * elixirs.value.length)
  const newElixir = elixirs.value[rnd]
  createElixirMessage.value = {
    effect: newElixir.effect,
    sideEffects: newElixir.sideEffects,
    characteristics: newElixir.characteristics
  }
}

function resetElixir() {
  createElixirIngredients.value = []
  createElixirMessage.value = ''
  
}




watch(selected, showElixir)
watch(selectedSpell, showSpell)
watch(selectedIngredient, showIngredients)


</script>

<!-- --- -->
<template>
  <main>
    <header>
      <nav>
        <h1 class="header-title">Wizard API</h1>
      </nav>
    </header>
    <section class="main-section">
      <div class="left-side"></div>
      <!-- Elixirs Group -->
      <div class="elixir-select-group">
        <h1 class="elixirs-main-title">Elixirs</h1>
        <select class="select-elixir" v-model="selected" @select="{ showElixir }">
          <option v-for="elixir in elixirs">
            {{ elixir.name }}
          </option>
        </select>
        <Elixir :singleElixir="singleElixir" />
      </div>

      <!-- Spell Group -->
      <div class="elixir-select-group">
        <h1 class="spells-main-title">Spells</h1>
        <select class="select-elixir" v-model="selectedSpell" @select="{ showSpell }">
          <option v-for="spell in spells">
            {{ spell.name }}
          </option>
        </select>
        <Spell :singleSpell="singleSpell" />
      </div>

      <!-- Ingredients Group -->
      <div class="elixir-select-group">
        <h1 class="ingredient-main-title">Ingredients</h1>
        <select class="select-elixir" v-model="selectedIngredient" @select="{ showIngredients }">
          <option v-for="ingredient in ingredients">
            {{ ingredient.name }}
          </option>
        </select>
        <Ingredient :singleIngredient="singleIngredient" />
        <ul v-for="ingredient in createElixirIngredients">
          <li>{{ ingredient }}</li>
        </ul>
        <div>
          <button @click="createElixir">Mix Ingredients</button>
          <button @click="resetElixir">Reset</button>
        </div>
        <div class="elixir-message">
          <h3 class="new-elixir-title" v-if="createElixirMessage.effect">New Elixir Created</h3>
          <p v-if="createElixirMessage.effect"><strong>Effect: </strong>{{
            createElixirMessage.effect ?
              createElixirMessage.effect : "Unknown"
          }}</p>
          <p v-if="createElixirMessage.characteristics"><strong>Characteristics: </strong>{{
            createElixirMessage.characteristics ? createElixirMessage.characteristics : "Unknown"
          }}</p>
          <p v-if="createElixirMessage.sideEffects"><strong>Side Effects: </strong>{{
            createElixirMessage.sideEffects ?
              createElixirMessage.sideEffects : "Unknown"
          }}</p>
        </div>
      </div>
      <div></div>

    </section>
    <footer>
      <div class="footer-title">By Kyle Larson, Jan. 2023</div>
    </footer>
  </main>

</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@400;700&family=Josefin+Sans:wght@300;400;700&family=Moon+Dance&display=swap');

/* ii {
  font-family: 'Moon Dance', cursive;
  font-family: 'Josefin Sans', sans-serif;
  font-family: 'Amatic SC', cursive;
} */

header {
  background-color: black;
  width: 100vw;
  height: 70px;
}

.header-title {
  font-family: 'Josefin Sans', sans-serif;
  font-size: 34px;
  margin: 0 0 0 20px;
  padding: 20px 0 0 0;
  color: white;
}

.main-section {
  display: grid;
  grid-template-columns: 300px auto auto auto 40px;
  gap: 10px;
}

.left-side {
  background-image: url(./assets/Wizard3.jpg);
  background-position: top;
  background-repeat: no-repeat;
  background-size: cover;
  grid-column: 1 / 2;
  height: 83vh;
}

footer {
  background-color: black;
  width: 100vw;
  height: 40px;
}

.footer-title {
  color: white;
  text-align: center;
  padding: 7px 0 5px 0;
}

.elixirs-main-title {
  font-family: 'Moon Dance', cursive;
  font-size: 48px;
  text-shadow: 1px 2px 6px rgb(13, 128, 0);
}

.spells-main-title {
  font-family: 'Moon Dance', cursive;
  font-size: 48px;
  text-shadow: 1px 2px 6px purple;
}

.ingredient-main-title {
  font-family: 'Moon Dance', cursive;
  font-size: 48px;
  text-shadow: 1px 2px 6px rgb(0, 84, 209);
}

.new-elixir-title {
  text-shadow: 1px 2px 6px rgb(0, 84, 209);
}

.select-elixir {
  margin: 0px;
  padding: 4px 6px;
  border-radius: 4px;
  background-color: #eee;
  display: block;
  width: 300px;
}

p {
  margin: 4px 0 0 4px;
}


button {
  padding: 5px 10px;
  background-color: darkblue;
  color: white;
  font-size: 16px;
  margin: 12px 8px 12px 0px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  display: inline-block;
}

button:hover {
  transform: scale(0.98);
}

.elixir-select-group {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
}

.elixir-details {
  margin-top: 12px;
  width: 325px;
  padding: 0 15px 0 8px;
}

.elixir-title {
  margin-top: -5px;
}

.spell-title {
  margin-top: -5px;
}

.elixir-message {
  max-width: 300px;
}


@media (max-width: 800px) {
  .main-section {
    grid-template-columns: auto;
    gap: 20px;
    justify-content: center;
  }

  .left-side {
    height: 53vh;
    width: 90vw;
  }
}

</style>
