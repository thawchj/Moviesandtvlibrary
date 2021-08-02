<template>
  <div class="body">
    <div
      v-if="movie"
      class="body-movie container-fluid"
      :style="{
        backgroundImage: `url(https://www.themoviedb.org/t/p/w1920_and_h800_multi_faces${movie.backdrop_path})`,
      }"
    >
      <nuxt-link to="/movies">
        <b-icon
          icon="arrow-left-short"
          font-scale="3"
          class="iconback mt-3"
        ></b-icon>
      </nuxt-link>
      <div class="row offset-lg-1 d-flex align-items-center">
        <div class="info-movie col-lg-4 p-5 col-md-12">
          <img
            class="img-movie"
            :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${movie.poster_path}`"
            alt=""
            width="100%"
          />
        </div>
        <div class="info-movie col-lg-6 col-md-12">
          <h1 class="name-movie mt-5 mb-4">{{ movie.title }}</h1>
          <div class="time-movies mb-2">
            <span class="text-time"
              ><b-icon icon="calendar-event" class="mr-2"></b-icon
              >{{ getDate(movie.release_date) }}</span
            >
            <span class="text-time"
              ><b-icon icon="clock" class="mr-2"></b-icon
              >{{ movie.runtime }}min</span
            >
            <button class="add-button" @click="addToList">
              <b-icon scale="1.5" icon="plus-circle" class="mr-2"></b-icon
              ><span class="ml-2">ADD TO LIST</span>
            </button>
          </div>
          <div class="row">
            <div v-for="f in movie.genres" :key="f.id">
              <div class="text-genres pl-3">{{ f.name }}</div>
            </div>
          </div>

          <p class="text-overview mt-2">{{ movie.overview }}</p>

          <div class="my-4 embed-responsive embed-responsive-16by9">
            <iframe
              v-if="
                movie.videos && movie.videos.results && movie.videos.results[0]
              "
              width="560"
              height="315"
              :src="`https://www.youtube.com/embed/${movie.videos.results[0].key}?controls=0`"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
            <div v-else></div>
          </div>
        </div>
        <div class="black-cover"></div>
      </div>
    </div>
    <div v-else>
      <pagenotfound></pagenotfound>
    </div>
    <footerlib></footerlib>
  </div>
</template>

<script>
import Footerlib from '~/components/Footerlib.vue'
import Pagenotfound from '~/components/Pagenotfound.vue'

export default {
  components: { Footerlib, Pagenotfound },

  async asyncData({ $axios, route }) {
    let res = null
    try {
      res = await $axios.$get(
        `https://api.themoviedb.org/3/movie/${route.params.id}?api_key=${process.env.API_KEY}&append_to_response=videos,images`
      )
    } catch (e) {}
    return {
      movie: res,
      moviemake: {
        type: 'movie',
      },
    }
  },
  head() {
    return {
      title: this.movie ? this.movie.title : 'movies',
      meta: [
        {
          hid: 'movies',
          name: 'movies',
          content: 'movies',
        },
      ],
    }
  },
  methods: {
    getDate(dsting){
      const dobj = new Date(dsting)
      return dobj.toLocaleDateString()
    },
    async addToList() {
      const item = {
        idmovie: this.movie.id,
        namemovie: this.movie.title,
        imgmovie: this.movie.poster_path,
        type: this.moviemake.type,
      }

      // 1. ดึงค่า cookie แล้วเช็คว่า มีตัวแปร  movie-selected ยัง
      const movieSelected = await this.getCookie('movie-selected')
      if (movieSelected === '') {
        // 1.1ถ้ายังไม่มี movie-selected ให้เพิ่มเข้าไป
        const itemJson = JSON.stringify([item])
        await this.setCookie('movie-selected', itemJson, 5) // เก็บข้อมูลใน cookie ชื่อ movie-selected
        this.makeToast('secondary', item.namemovie)
        return
      }

      // 1.2 ถ้ามี movie-selected แล้วให้เช็คว่า มี หนังที่เลือกอยู่ใน movie-selected แล้วยัง
      const movieSelectedArr = JSON.parse(movieSelected)

      const checkhavemovieselected = movieSelectedArr.some(
        (ms) => ms.idmovie === item.idmovie
      )

      // 1.2.1 ถ้ายังไม่มีที่เลือกใน movie-selected ให้ push หนังเข้าไป ใน movie-selected ได้เลย
      if (checkhavemovieselected === false) {
        movieSelectedArr.push(item)
        const movieSelectedArrJson = JSON.stringify(movieSelectedArr)
        await this.setCookie('movie-selected', movieSelectedArrJson, 5)
      }
      this.makeToast('secondary', item.namemovie)
    },
    makeToast(variant = null, name) {
      this.$bvToast.toast(name, {
        title: 'Add to list Success',
        variant,
        solid: true,
      })
    },
    setCookie(cname, cvalue, exdays) {
      const d = new Date()
      d.setTime(d.getTime() + exdays * 24 * 60 * 60 * 1000)
      const expires = 'expires=' + d.toUTCString()
      document.cookie = cname + '=' + cvalue + ';' + expires + ';path=/'
    },
    getCookie(cname) {
      const name = cname + '='
      const decodedCookie = decodeURIComponent(document.cookie)
      const ca = decodedCookie.split(';')
      for (let i = 0; i < ca.length; i++) {
        let c = ca[i]
        while (c.charAt(0) === ' ') {
          c = c.substring(1)
        }
        if (c.indexOf(name) === 0) {
          return c.substring(name.length, c.length)
        }
      }
      return ''
    },
  },
}
</script>

<style scoped>
.body {
  background-color: black;
}
.iconback {
  position: absolute;
  z-index: 3;
  color: white;
}
.iconback:hover {
  color: rgb(190, 190, 190);
}
.body-movie {
  background: black;
  background-size: cover;
  background-position: top;
  background-repeat: no-repeat;
  position: relative;
}

.text-time {
  font-size: 20px;
  line-height: 250%;
}
.text-genres {
  font-size: 20px;
}
.add-button {
  font-size: 20px;
  margin: 0px 5px 0px 5px;
  background-color: rgba(54, 54, 54, 0.5);
  border: 2px solid white;
  padding: 8px 10px;
  color: white;
  border-radius: 2px;
}
.add-button:hover{
  color: rgb(190, 190, 190);
  border: 2px solid rgb(190, 190, 190);
}

.info-movie {
  color: rgb(255, 255, 255);
  z-index: 2;
}
.name-movie {
  font-weight: bold;
  font-size: 50px;
}
.text-overview {
  font-size: 18px;
}

.black-cover {
  background-color: rgba(0, 0, 0, 0.705);
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 1;
}
</style>