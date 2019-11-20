<template>
  <div class="popular-films films">
    <popup
      @close-popup="popupClose"
      :show="showPopup"
      :filmId="currentFilm.id"
      :filmName="currentFilm.name"
    />
    <h2 class="films__title">Популярные фильмы</h2>

    <div class="films__filter">
      <span>Выберите жанр: </span>
      <select name="genre" id="genre" class="films__filter__genre" v-model="selectedGenreId">
        <option
            v-for="(genre,index) in genresList"
            :key="index"
            :value="genre.id"
        >{{genre.name}}</option>
      </select>
    </div>


    <div v-if="popularFilms.length !== 0 && selectedGenreId === ''" class="films__list">
      <film
        v-for="(film, index) in popularFilms"
        :key="index"
        :title="film.title"
        :desc="film.overview"
        :source="'http://image.tmdb.org/t/p/w400' + film.poster_path"
        :alt_desc="film.title"
        :id="film.id"
        @open-popup="updatePopup"
      />
    </div>

    <div v-else-if="filteredPopularFilms.length !== 0 && selectedGenreId.name !== ''" class="films__list">
      <film
          v-for="(film, index) in filteredPopularFilms"
          :key="index"
          :title="film.title"
          :desc="film.overview"
          :source="'http://image.tmdb.org/t/p/w400' + film.poster_path"
          :alt_desc="film.title"
          :genres="film.genres"
          :id="film.id"
          @open-popup="updatePopup"
      />
    </div>
    
    <div v-else class="films__wait">
      <img src="../assets/Gear-0.3s-200px.svg" alt="Waiting...">
      <span class="films__wait__msg">{{ messageWait }}</span>
    </div>

    <ul class="errors">
      <li
        v-for="(error, index) in errors"
        :key="index"
      >
        {{error.message}}
      </li>
    </ul>
  </div>
</template>

<script>
  /* eslint-disable no-console */

  import Film from "./Film";
  import axios from "axios"
  import Popup from "./Popup";

  export default {
    name: "Popular",
    components: {Popup, Film},

    data () {
      return {
        popularFilms: [],
        errors: [],
        showPopup: false,
        popupFilmName: '',
        currentFilm: {
          id: '',
          name: '',
        },
        genresList: [],
        selectedGenreId: '',
      }
    },

    created: function () {
      axios({
        url: "https://api.themoviedb.org/3/movie/popular?page=1&language=ru&api_key=9063ae2a574c85c70476669d90a3a4a9&region=ru",
        method: "GET",
        headers: {},
        data: {}
      })
        .then(response => {
          // eslint-disable-next-line no-console
          console.log(response);
          this.popularFilms = response.data.results;
        })
        .catch(error => {
          this.errors.push(error);
          // eslint-disable-next-line no-console
          console.log(error);
        });

      axios({
        url: "https://api.themoviedb.org/3/genre/movie/list?api_key=9063ae2a574c85c70476669d90a3a4a9&language=ru",
        method: "GET",
        headers: {},
        data: {}
      })
        .then(response => {
          // eslint-disable-next-line no-console
          console.log(response);
          this.genresList = response.data.genres;
        })
        .catch(error => {
          this.errors.push(error);
          // eslint-disable-next-line no-console
          console.log(error);
        });
    },

    computed: {
      filteredPopularFilms () {
        return this.popularFilms.filter( film => {
          return film.genre_ids.includes(this.selectedGenreId);
        })
      },

      messageWait () {
        if (this.popularFilms.length === 0) {
          return 'Загрузка фильмов, подождите...'
        }
        else if (this.filteredPopularFilms.length === 0) {
          return 'Фильмы с указанным жанром не найдены, выберите другой...'
        }
        else return 'Подождите, прогружаем фильмы...';
      }

    },

    methods: {
      arrayIntersectionExist (array1, array2) {
        let arrIntersection = array1.filter( item1 => {
          array2.includes(item1)
        });

        return Boolean(arrIntersection.length)
      },
      updatePopup (filmId, filmName) {
        console.log('Open popup for film with id:' + filmId);
        this.currentFilm.id = filmId;
        this.currentFilm.name = filmName;
        this.popupOpen();
      },

      popupOpen () {
        !this.showPopup ? this.showPopup = !this.showPopup : ''
      },

      popupClose () {
        this.showPopup ? this.showPopup = !this.showPopup : '';
        // console.log('Close popup');
      },

    },
  }
</script>

<style lang="scss">
  .films {
    padding: 60px 0;
    &__list {
      margin-top: 60px;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-column-gap: 30px;

      &__item {

      }
    }

    &__wait {
      margin-top: 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 30px;
      font-weight: 600;
      color: #c2ff62;

      &__msg {
        margin-left: 20px;
      }
    }
  }
</style>