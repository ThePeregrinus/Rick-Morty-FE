<script setup>
import { ref } from 'vue'

const props = defineProps({
  name: String,
  status: String,
  location: String,
  image: String,
  episodes: Array
})

const episode = ref(null)
async function fetchData() {
  episode.value = null
  const res = await fetch(props.episodes[0])
  episode.value = await res.json()
}
fetchData()
</script>

<template>
  <article class="card">
    <div class="img-wrapper">
      <img class="picture" :src="image" loading="lazy" />
    </div>
    <div class="info">
      <div class="section">
        <h2>{{ name }}</h2>
        <span class="status"><span class="icon" :class="props.status"></span>{{ status }}</span>
      </div>

      <div class="section">
        <span class="gray">Last known location: </span>
        <span>{{ location }}</span>
      </div>
      <div class="section">
        <span class="gray">First seen in:</span>
        <span>{{ episode?.name || 'Loading...' }}</span>
      </div>
    </div>
  </article>
</template>

<style scoped>
.card {
  display: flex;

  width: 500px;
  height: 230px;
  background-color: #3c3e44;
  color: #fff;

  margin: 0.7rem;
  border-radius: 0.7rem;

  overflow: hidden;
}
.picture {
  object-fit: cover;
  height: 100%;
  width: 100%;
}
.info {
  display: flex;
  flex-direction: column;
  flex: 3 1 0%;

  padding: 0.7rem;
}

.section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex-grow: 1;
}

.status {
  display: flex;
  align-items: center;
}

.icon {
  height: 0.5rem;
  width: 0.5rem;

  border-radius: 50%;

  margin-right: 0.5rem;

  background: #d1ded1;
}
.icon.Alive {
  height: 0.5rem;
  width: 0.5rem;

  border-radius: 50%;

  margin-right: 0.5rem;

  background: #319a46;
}

.icon.Dead {
  height: 0.5rem;
  width: 0.5rem;

  border-radius: 50%;

  margin-right: 0.5rem;

  background: #ff6347;
}

.img-wrapper {
  flex: 2 1 0%;
}

.gray {
  color: #808080;
  font-weight: 500;
}
h2 {
  font-size: 1.5rem;
  font-weight: 800;
}
</style>
