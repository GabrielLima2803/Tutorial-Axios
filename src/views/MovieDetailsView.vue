<script setup>
import { defineProps, onMounted } from 'vue'
import { useMovieStore } from '@/stores/movie'
const movieStore = useMovieStore()

const props = defineProps({
  movieId: {
    type: Number,
    required: true
  }
})

onMounted(async () => {
  await movieStore.getMovieDetail(props.movieId)
})
</script>

<template>
  <div class="main">
    <div class="content">
      <img
        :src="`https://image.tmdb.org/t/p/w185${movieStore.currentMovie.poster_path}`"
        :alt="movieStore.currentMovie.title"
      />

      <div class="details">
        <h1>Filme: {{ movieStore.currentMovie.title }}</h1>
        <p>{{ movieStore.currentMovie.tagline }}</p>
        <p>{{ movieStore.currentMovie.overview }}</p>
        <p>Orçamento: ${{ movieStore.currentMovie.budget }}</p>
        <p>Avaliação: {{ movieStore.currentMovie.vote_average }}</p>
      </div>
    </div>
    <p>Produtoras</p>
    <div class="companies">
      <template v-for="company in movieStore.currentMovie.production_companies" :key="company.id">
        <img
          v-if="company.logo_path"
          :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`"
          :alt="company.name"
        />
        <p v-else>{{ company.name }}</p>
      </template>
    </div>
  </div>
</template>

<style scoped>
.main {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f8f8;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

.content {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

img {
  max-width: 185px;
  border-radius: 8px;
  margin-right: 20px;
}

.details {
  flex: 1;
}

h1 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #333;
}

p {
  margin-bottom: 8px;
  color: #666;
}

.movie-card {
  display: flex;
  flex-direction: column;
  background-color: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.movie-card img {
  width: 100%;
  border-radius: 8px 8px 0 0;
}

.movie-details {
  padding: 20px;
}

.companies {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.companies img {
  max-width: 50px;
  border-radius: 8px;
}
</style>
