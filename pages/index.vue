<template>
  <main class="main">
    <!-- Hero Section -->
    <HeroSection />

    <!-- Search -->
    <div class="search container">
      <input @keyup.enter="$fetch" type="text" placeholder="Search" v-model.lazy="searchInput">
      <button @click="clearSearch" v-show="searchInput !== ''" class="btn">Clear Search</button>
    </div>

    <!-- Loadder -->
    <TheLoader v-if="$fetchState.pending" />

    <!-- Movie Grid -->
    <section v-else class="movies container">
      <div v-if="searchInput === ''" class="movies-grid" id="movies-grid">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`" alt="">
            <p class="movie-review">{{ movie.vote_average }}</p>
            <p class="movie-overview">{{ movie.overview }}</p>
          </div>
          <div class="movie-info">
            <p class="title">{{ movie.title.slice(0, 25) }}<span v-if="movie.title.length > 25">...</span></p>
            <p class="release">
              Released: {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink class="btn-secondary" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">Get More Info</NuxtLink>
          </div>
        </div>
      </div>

  <!-- Searched Movies -->
       <div v-else class="movies-grid" id="movies-grid">
        <div class="movie" v-for="(movie, index) in searchedMovies" :key="index">
          <div class="movie-img">
            <img :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`" alt="">
            <p class="movie-review">{{ movie.vote_average }}</p>
            <p class="movie-overview">{{ movie.overview }}</p>
          </div>
          <div class="movie-info">
            <p class="title">{{ movie.title.slice(0, 25) }}<span v-if="movie.title.length > 25">...</span></p>
            <p class="release">
              Released: {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink class="btn-secondary" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">Get More Info</NuxtLink>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import axios from 'axios'

export default {
  head() {
    return {
      title: 'Movie App',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Check out all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, streaming',
        },
      ],
    }
  },

  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: ''
    }
  }, 
  async fetch() {
    if(this.searchInput === '') {
      await this.getMovies()
      return
    } 
    if(this.searchInput !== '') {
      await this.searchMovies()
    }
  },

  fetchDelay: 3000,

  methods: {
    async getMovies() {
      const data = axios.get('https://api.themoviedb.org/3/movie/now_playing?api_key=dddbb43fc26e555ec310b056f92f5841&language=en-US&page=1')
      const result = await data
      result.data.results.forEach(movie => {
        this.movies.push(movie)
      })
    },

    async searchMovies() {
      const data = axios.get(`https://api.themoviedb.org/3/search/movie?api_key=dddbb43fc26e555ec310b056f92f5841&language=en-US&page=1&query=${this.searchInput}`)
      const result = await data
      result.data.results.forEach(movie => {
        this.searchedMovies.push(movie)
      })
    },

    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    }
  },
}
</script>

<style lang="scss">
.main {
  
  .search {
    display: flex;
    padding: 2rem 3rem;

    input {
      max-width: 20rem;
      width: 100%;
      padding: 1rem 1.25rem;
      font-size: 1rem;
      border: none;
      border-radius: 4px 0 0 4px;

      &:focus {
        outline: none;
      }
    }
    .btn {
      font-size: 1rem;
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }

  }
  .movies {
    padding: 0 3rem;

    .movies-grid {
      display: grid;
      gap: 2em 0.5em;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      place-items: center;
      
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;

        .movie-img {
          position: relative;
          overflow: hidden;
          margin-bottom: 1.25rem;

          &:hover {

            .movie-overview {
              transform: translateY(0);
            }
          }

          .movie-review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 3rem;
            height: 3rem;
            background-color: $accent;
            color: $white;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .movie-overview {
            position: absolute;
            bottom: 0;
            background-color: $brown;
            color: $white;
            line-height: 1.6rem;
            padding: 1rem;
            transform: translateY(100%);
            transition: transform 0.2s ease;
          }
        }
        .movie-info {
          
          & > * {
            margin-bottom: 1.25rem;
          }
          .title {
            font-size: 1.25rem;
          }
          .release {
            font-size: 0.85rem;
            color: darken($white, 30%);
          }
        }
      }
    }
  }
}

</style>
