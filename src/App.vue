<template>
  <main class="app">
    <div class="app__container">
      <div class="app__form">
        <input type="text" @keypress.enter="fetchMovies" v-model="queryTitle" placeholder="Title...">
        <input type="text" @keypress.enter="fetchIMDb" v-model="queryIMDb" placeholder="IMDb - For more info">
      </div>
      <TransitionGroup tag="div" name="fade">
        <div class="searchResult">
          <div class="app__result result">
            <div class="result__container" v-for="item in movies.Search" :key="item.id">
              <div class="result__img">
                <a :href="item.Poster" target="_blank">
                  <img :src="item.Poster">
                </a>
              </div>
              <div class="result__content">
                <h1 class="result__title">
                  {{ item.Title }}
                </h1>
                <h1 class="result__year">
                  Released: {{ item.Year }}-year
                </h1>
                <h1 class="result__id">
                  IMDb: {{ item.imdbID }}
                </h1>
                <h1 class="result__type">
                  Type: {{ item.Type.toUpperCase() }}
                </h1>
              </div>
            </div>
          </div>
          <div v-if="movies.Error">
            <h1 class="error">Not Found!</h1>
          </div>
          <div class="app__result result" v-if="id">
            <div class="result__container" v-for="item in result" :key="item.id">
              <div class="result_id" v-if="item.Response == 'True'">
                <div class="result__img">
                  <a :href="item.Poster" target="_blank">
                    <img :src="item.Poster">
                  </a>
                </div>
                <div class="result__content">
                  <h1 class="result__title">
                    {{ item.Title }}
                  </h1>
                  <br>
                  <h1 class="result__released">
                    Released: {{ item.Released }}-year
                  </h1>
                  <br>
                  <h1 class="result__genre">
                    Genre: {{ item.Genre }}
                  </h1>
                  <br>
                  <h1 class="result__rated">
                    Rated: {{ item.Rated }}-year
                  </h1>
                  <br>
                  <h1 class="result__type">
                    Type: {{ item.Type.toUpperCase() }}
                  </h1>
                  <br>
                  <h1 class="result__runtime">
                    Runtime: {{ item.Runtime }}
                  </h1>
                  <br>
                  <h1 class="result__language">
                    Language: {{ item.Language }}
                  </h1>
                  <br>
                  <h1 class="result__score" style="color: yellow;">
                    Metascore: {{ item.Metascore }}
                  </h1>
                  <br>
                </div>
              </div>
              <div v-else class="error">
                <h1>Incorrect IMDb ID.</h1>
              </div>
            </div>
          </div>
        </div>
      </TransitionGroup>
    </div>
  </main>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    apiKey: 'YOUR_API_KEY',
    queryTitle: '',
    queryIMDb: '',
    movies: {},
    id: false,
    searchIcon: './public/search.svg',
    result: {},
  }),
  methods: {
    fetchMovies() {
      this.id = false
      axios(`https://www.omdbapi.com/?apikey=${this.apiKey}&s=${this.queryTitle}&plot=full`)
        .then(response => this.movies = response.data)
        .catch(err => alert(`Error, ${err}`))
    },
    fetchIMDb() {
      this.movies = {}
      this.id = true
      axios(`https://www.omdbapi.com/?apikey=${this.apiKey}&i=${this.queryIMDb}&plot=full`)
        .then(response => this.result[0] = response.data)
        .catch(err => alert(`Error, ${err}`))

    },
  }
}
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css?family=Roboto:regular,900);

*,
*::before,
*::after {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Roboto";
  font-weight: 400;
}

.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01);
}

.fade-leave-active {
  position: absolute;
}

.error {
  width: 100%;
  color: red;
  text-align: center;
  text-transform: uppercase;
}

.app {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 30px 0px 0px 0px;
  background: #00000060;

  &__container {
    width: 800px;
    margin: 0 auto;
    padding: 30px 50px;
    border-radius: 30px 20px;
    background: rgb(2, 0, 36);
    background: linear-gradient(90deg, rgb(24, 22, 59) 0%, rgba(9, 9, 121, 1) 35%, rgba(0, 212, 255, 1) 100%);
  }

  &__form {
    display: flex;
    align-items: center;
    justify-content: space-between;

    input {
      background: #fff;
      border: none;
      outline: none;
      font-size: 24px;
      width: 90%;
      height: 40px;
      padding: 0px 10px;
      border-radius: 10px 15px;
      text-align: right;
      margin-right: 10px;
    }

    button {
      width: 50px;
      border: none;
      background: inherit;
      cursor: pointer;
      height: 50px;
    }
  }

  .result {
    margin: 30px 0;

    &__container,
    &_id {
      display: flex;
      align-items: center;
      width: 100%;
      justify-content: space-between;
    }

    &__img {
      img {
        width: 300px;
        height: 300px;
      }
    }

    &__content {
      text-align: center;
      color: #fff;
    }

    &__title {
      font-size: 40px;
      text-transform: uppercase;

    }

    &__year {
      font-size: 24px;
    }
  }

}
</style>