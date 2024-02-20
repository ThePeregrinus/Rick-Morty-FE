<script setup>
import { ref, watch } from 'vue'

import CharacterCard from './CharacterCard.vue'

const id = ref(1)

const heroes = ref(null)
const isFiltered = ref(false)
const name = ref('')
const status = ref('Alive')

function toPreviousPage() {
  id.value -= 6
}

function toNextPage() {
  id.value += 6
}

function filter() {
  id.value = 1
  id.name = ''
  id.status = 'Alive'

  isFiltered.value = !isFiltered.value
}

watch(id, () => fetchData(id))
watch(isFiltered, () => fetchFilterData())

async function fetchData() {
  heroes.value = null

  const res = await fetch(
    `https://rickandmortyapi.com/api/character/${id.value},${id.value + 1},${id.value + 2},${id.value + 3},${id.value + 4},${id.value + 5}`
  )
  heroes.value = await res.json()
}

async function fetchFilterData() {
  heroes.value = null

  const res = await fetch(
    `https://rickandmortyapi.com/api/character/?name=${name.value}&status=${status.value}`
  )
  heroes.value = (await res.json()).results
}

fetchData(id)
</script>

<template>
  <div class="menu">
    <button
      @click="toPreviousPage"
      :disabled="id === 1 || !heroes"
      class="disabled? || !isFiltered"
    >
      Previous
    </button>
    <button @click="toNextPage" :disabled="heroes?.length < 6 || isFiltered">Next</button>

    <input placeholder="Name" v-model="name" />
    <select placeholder="Status" v-model="status">
      <option value="Alive">Alive</option>
      <option value="Dead">Dead</option>
      <option value="unknown">unknown</option>
    </select>
    <button @click="filter">{{ isFiltered ? 'Reset' : 'Filter' }}</button>
  </div>
  <div class="grid">
    <div v-for="hero in heroes" :key="hero.id">
      <CharacterCard
        :name="hero.name"
        :status="hero.status"
        :location="hero.location.name"
        :image="hero.image"
        :episodes="hero.episode"
      />
    </div>
  </div>
</template>

<style scoped>
.grid {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
.menu {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 1rem;
}

button {
  background-color: #04aa6d; /* Green */
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 1rem;
}

button[disabled] {
  background-color: #e7e7e7;
}
</style>
