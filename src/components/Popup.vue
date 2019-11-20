<template>
  <div v-if="show" class="popup" @keydown.esc="closePopup">
    <div
        @click="closePopup"
        class="popup__close"
    ></div>

    <div class="test-container">
      <div class="popup__content">
        <h1>{{filmName}}</h1>

        <div v-if="status === 'ready'" class="popup__film">
          <div class="popup__film__trailer">
            <iframe v-if="videoInfo.length !== 0"
                :src="sourceVideoYoutube"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
            </iframe>
            <div v-else class="popup__film__trailer popup__film__trailer--none">
              Видео не было найдено
            </div>
          </div>

          <div class="popup__film__wrapper">
            <div class="popup__film__content">
              <b>Информация о фильме:</b>
              <div class="popup__film__title" v-if="filmInfo.title">
                Название: {{filmInfo.title}}
              </div>
              <div class="popup__film__tagline" v-if="filmInfo.tagline">
                Слоган: {{filmInfo.tagline}}
              </div>
              <div class="popup__film__genres">
                Жанр: {{ getGenres }}
              </div>
              <div class="popup__film__status" v-if="filmInfo.status">
                Статус: {{filmInfo.status}}
              </div>
              <div class="popup__film__date" v-if="filmInfo.release_date">
                Дата выхода фильма: {{filmInfo.release_date}}
              </div>
              <div class="popup__film__runtime" v-if="filmInfo.runtime">
                Длительность: {{filmInfo.runtime}} мин.
              </div>
              <div class="popup__film__countries">
                Страна: {{ getCountries }}
              </div>
              <div class="popup__film__companies">
                Производство: {{ getCompanies }}
              </div>
              <div class="popup__film__budget" v-if="filmInfo.budget">
                Бюджет фильма: {{filmInfo.budget}} $
              </div>
              <div class="popup__film__average" v-if="filmInfo.vote_average">
                Оценка: {{filmInfo.vote_average}}/10
              </div>
            </div>
          </div>

        </div>

        <div v-if="status === 'wait'" class="popup__film popup__film--wait">
          <img src="../assets/Gear-0.3s-200px.svg" alt="Waiting...">
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  /* eslint-disable no-console */

  import axios from "axios";

  export default {
    name: "Popup",
    props: ['show', 'filmId', 'filmName'],
    data () {
      return {
        filmInfo: {},
        videoInfo: {},
        errors: [],
        status: '',
      }
    },
    computed: {
      sourceVideoYoutube () {
        return `https://www.youtube.com/embed/${this.videoInfo[0].key}`
      },

      getGenres () {
        let genresStr = '';

        this.filmInfo.genres.forEach((genre, index) => {
          index !== this.filmInfo.genres.length-1 ? genresStr += `${genre.name}, ` : genresStr += `${genre.name}`
        });

        return genresStr;
      },

      getCountries () {
        let countriesStr = '';

        this.filmInfo.production_countries.forEach((country, index) => {
          index !== this.filmInfo.production_countries.length-1 ? countriesStr += `${country.name}, ` : countriesStr += `${country.name}`
        });

        return countriesStr;
      },

      getCompanies () {
        let companiesStr = '';

        this.filmInfo.production_companies.forEach((company, index) => {
          index !== this.filmInfo.production_companies.length-1 ? companiesStr += `${company.name}, ` : companiesStr += `${company.name}`
        });

        return companiesStr;
      },

    },
    methods: {
      closePopup () {
        console.log('Emit close popup');
        this.$emit('close-popup')
      },

      getFilmInfo() {
        console.log('Getting film info');
        this.status = 'wait';

        //Get film INFO
        axios({
          url: `https://api.themoviedb.org/3/movie/${this.filmId}?language=ru&api_key=9063ae2a574c85c70476669d90a3a4a9`,
          method: "GET",
          headers: {},
          data: {}
        })
          .then(response => {
            console.log(response);
            this.filmInfo = response.data;
            this.status = 'ready';
          })
          .catch(error => {
            this.errors.push(error);
            console.log(error);
          });

        //Get film VIDEO
        axios({
          url: `https://api.themoviedb.org/3/movie/${this.filmId}/videos?api_key=9063ae2a574c85c70476669d90a3a4a9&language=ru`,
          method: "GET",
          headers: {},
          data: {}
        })
          .then(response => {
            console.log(response);
            this.videoInfo = response.data.results;
            this.status = 'ready';
          })
          .catch(error => {
            this.errors.push(error);
            console.log(error);
          });
      }
    },

    watch: {
      show () {
        if(this.show) {
          this.getFilmInfo();
        }
        else {
          console.log('No getting');
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(32, 32, 32, 0.9);
    color: #ffffff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 100;

    &__close {
      width: 40px;
      height: 40px;
      position: absolute;
      top: 3%;
      right: 3%;
      background: url("../assets/close-svg.svg") no-repeat;
      background-size: cover;
      cursor: pointer;
    }

    &__content {
      padding: 10% 0;
    }

    &__film {
      &__trailer {
        padding: 0 70px;
        height: 560px;
        margin-bottom: 40px;

        iframe {
          width: 100%;
          height: 100%;
        }
      }
      &__wrapper {
        padding: 0 70px;
      }
      &__content {
        padding: 20px 10px;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        background: #ffffff;
        color: #000;

        &>* {
          line-height: 140%;
        }
      }
    }
  }


</style>