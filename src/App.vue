<template>
  <section id="app">
    <h1>Movie Challenge</h1>
    <h4>Top Filmes by themoviedb</h4>
      <!--<select v-model="selected" class="order-select">
        <option>Ascending</option>
        <option>Descending</option>
      </select>-->
      <section class="movies">
        <div v-for="movie, i in movies" :key="i" class="movie" id="movie">
          <h3>{{ movie.title }}</h3>
          <div class="cover" id="cover">
            <img v-bind:src="`https://image.tmdb.org/t/p/w500${ movie.poster_path }`" v-on:mouseenter="show = true" v-on:mouseout="show = false" />
            <p>{{ movie.release_date }}</p>
          </div>
          <transition name="fade">
            <div v-if="show" class="overview">{{ movie.overview }}</div>
          </transition>
        </div>
      </section>
  </section>
</template>

<script>
import { onMounted, defineComponent, ref, nextTick } from "vue";
import api from "./services/api.js";
import { test } from "./scripts/index.js"

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
  },

  el: 'movie',
  data(){
    return{
      show: false
    }
  },

  methods: {
    async increment() {
      await nextTick()
      test()
    }
  }

});

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');

* {
  padding: 0;
  margin: 0;
}

#app {
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2rem;
}

.order-select {
  font-family: 'Poppins', sans-serif;
  margin-top: 1rem;
  padding: 0.2rem;
  border: none;
  border-radius: 0.2rem;
  background-color: #2c3e50;
  color: #fff;
  cursor: pointer;
}
.movies {
  margin-top: 2rem;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}

.movies h3 {
  font-size: 1.2rem;
}

.movie {
  width: 12rem;
  /* height: 18rem; */
  margin: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  gap: 0.2rem;
}

.cover {
  width: 100%;
  cursor: pointer;
}

.cover p {
  font-size: 0.8rem;
}

.cover img {
  width: 68%;
  border-radius: 0.6rem;
}

.overview {
  font-size: 0.8rem;
  text-align: left;
  position: relative;
}

.fade-enter-active {
  transition: all .3s ease;
}

.fade-leave-active {
  transition: all .4s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

.fade-enter,
.fade-leave-to
  {
  transform: translateY(-30px);
  opacity: 0;
}
</style>
