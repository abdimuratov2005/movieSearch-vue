<template>
  <main class="app">
    <div class="app__container">
      <div class="app__form">
        <input type="text" v-model="queryTitle" placeholder="Title...">
        <button @click="fetchMovie">
          <img :src="searchIcon">
        </button>
      </div>
      <TransitionGroup tag="div" name="fade">
        <div class="app__result result" v-for="item in movie.Search" :key="item.id">
          <div class="result__container">
            <div class="result__img">
              <img :src="item.Poster" alt="">
            </div>
            <div class="result__content">
              <h1 class="result__title">
                {{ item.Title }}
              </h1>
              <h4 class="result__year">
                Released: {{ item.Year }}-year
              </h4>
              <h4 class="result__id">
                ID: {{ item.imdbID }}
              </h4>
              <h4 class="result__type">
                Type: {{ item.Type }}
              </h4>
            </div>
          </div>
        </div>
        <div v-if="movie.Error">
          <h1 class="error">Not Found!</h1>
        </div>
      </TransitionGroup>
    </div>
  </main>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    apiKey: 'ec04f4d0',
    queryTitle: 'batman',
    movie: {},
    searchIcon: './public/search.svg',
  }),
  methods: {
    fetchMovie() {
      axios(`https://www.omdbapi.com/?apikey=${this.apiKey}&s=${this.queryTitle}&plot=full`)
        .then(response => this.movie = response.data)
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

/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}

.fade-leave-active {
  position: absolute;
}

.error{
  color: red;
  text-align: center;
}

.app {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 30px 0px 0px 0px;

  &__container {
    width: 800px;
    margin: 0 auto;
    padding: 30px 50px;
    border-radius: 30px 20px;
    background: rgb(2, 0, 36);
    background: linear-gradient(90deg, rgba(2, 0, 36, 1) 0%, rgba(9, 9, 121, 1) 35%, rgba(0, 212, 255, 1) 100%);
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

    &__container {
      display: flex;
      align-items: center;
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