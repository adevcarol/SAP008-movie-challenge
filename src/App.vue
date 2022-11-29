<template>
  <section id="app">
    <h1>Movie Challenge</h1>
      <div class="movies">
        <div v-for="movie, i in movies" :key="i" class="movie">
          <p>{{ movie.title }}</p>
          <img src="https://image.tmdb.org/t/p/w500/od22ftNnyag0TTxcnJhlsu3aLoU.jpg" />
        </div>
      </div>
  </section>
</template>

<script>
import { onMounted, defineComponent, ref } from "vue";
import api from "./services/api.js";

export default defineComponent({
  name: 'App',
  setup(){
    const movies = ref([]);
    const fetchMovies = () => api.get("/movie/top_rated?api_key=c32b068fe2d47f0561851d0df1792423&language=en-US&page=1")
      .then((resp) => {
        console.log(resp);
        movies.value = resp.data.results
      });
    onMounted(fetchMovies);
    return { movies }
  }
});
</script>

<style>
* {
  padding: 0;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2rem;
}

.movies {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}

.movie {
  width: 12rem;
  margin: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

img {
  width: 68%;
  border-radius: 0.6rem;
}
</style>
