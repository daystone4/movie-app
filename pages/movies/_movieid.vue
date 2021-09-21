<template>
    <TheLoader v-if="$fetchState.pending" />
    <div v-else class="single-movie">
        <NuxtLink class="btn" :to="{name: 'index'}">Go back</NuxtLink>
        <div class="movie-info">
            <div class="movie-img">
                <img :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`" alt="">
            </div>
            <div class="movie-content">
                <h1>Title: {{  movie.title }}</h1>
                <p><span>TagLine: </span> "{{ movie.tagline }}"</p> 
                <p><span>Released:</span>
                    Released: {{
                    new Date(movie.release_date).toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                })
              }}
                </p> 
                <p><span>Duration:</span> {{ movie.runtime }} minutes</p>
                <p class="movie-fact">
                <span>Revenue:</span>
                {{
                        movie.revenue.toLocaleString('en-us', {
                        style: 'currency',
                        currency: 'USD',
                    })
          }}
        </p>
        <p><span>Overview: </span> {{ movie.overview }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    head() {
        return {
        title: this.movie.title,
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
            movie: '',
        }
    },

    async fetch() {
        await this.getSingleMovie()
    },

    fetchDelay: 2000,

    methods: {
        async getSingleMovie() {
            const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=dddbb43fc26e555ec310b056f92f5841&language=en-US`)
            const result = await data
            this.movie = result.data
        }
    }
}
</script>

<style lang="scss" scoped>
.single-movie {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem 4rem;

  .btn {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    color: $white;

    .movie-img {
      img {
        max-height: 40rem;
      }
    }
    .movie-content {

        & > * {
            margin-top: 0.875rem;
        }

        h1 {
            font-size: clamp(2.5rem, 3vw + 1rem, 3.5rem);
        }

        p {
            font-size: clamp(1rem, 2vw, 1.25rem);
            line-height: 1.6rem;

            span {
            font-weight: 600;
            text-decoration: underline;
            }
        }
    }
  }

@media screen and (min-width: 40em) {
        .movie-info {
            flex-direction: row;
            align-items: flex-start;
        }
       
    }
}
</style>