<template>
  <div class="body">
    <div
      v-if="tvshow"
      class="body-tvshows container-fluid"
      :style="{
        backgroundImage: `url(https://www.themoviedb.org/t/p/w1920_and_h800_multi_faces${tvshow.backdrop_path})`,
      }"
    >
      <nuxt-link to="/tvshows">
        <b-icon
          icon="arrow-left-short"
          font-scale="3"
          class="iconback mt-3"
        ></b-icon>
      </nuxt-link>

      <div class="row offset-lg-1 d-flex align-items-center">
        <div class="info-tvshows col-lg-4 p-5 col-md-12">
          <img
            class="img-tvshows"
            :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${tvshow.poster_path}`"
            alt=""
            width="100%"
          />
        </div>
        <div class="info-tvshows col-lg-6 col-md-12">
          <h1 class="name-tvshows mt-5 mb-3">{{ tvshow.name }}</h1>
          <div class="time-tvshows my-2">
            <span class="text-time"
              ><b-icon icon="calendar-event" class="mr-2"></b-icon
              >{{ getDate(tvshow.first_air_date) }}</span
            >
            <span class="text-time"
              ><b-icon icon="clock" class="mr-2"></b-icon
              >{{ tvshow.number_of_seasons }} seasons</span
            >
            <span class="text-time"
              >{{ tvshow.number_of_episodes }} episodes</span
            >
            <button class="add-button" @click="addToList">
              <b-icon scale="1.5" icon="plus-circle" class="mr-2"></b-icon
              ><span class="ml-2">ADD LIST</span>
            </button>
          </div>
          <div class="row ">
            <div v-for="m in tvshow.genres" :key="m.id">
              <div class="text-genres pl-3">{{ m.name }}</div>
            </div>
          </div>

          <p class="text-overview mt-2">{{ tvshow.overview }}</p>
          <div class="my-4 embed-responsive embed-responsive-16by9">
            <iframe
              v-if="
                tvshow.videos &&
                tvshow.videos.results &&
                tvshow.videos.results[0]
              "
              width="560"
              height="315"
              :src="`https://www.youtube.com/embed/${tvshow.videos.results[0].key}?controls=0`"
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
        `https://api.themoviedb.org/3/tv/${route.params.id}?api_key=${process.env.API_KEY}&append_to_response=videos,images`
      )
    } catch (e) {}
    return {
      tvshow: res,
      tvshowmake: {
        type: 'tvshow',
      },
    }
  },
  head() {
    return {
      title: this.tvshow ? this.tvshow.name : 'tvshows',
      meta: [
        {
          hid: 'tvshows',
          name: 'tvshows',
          content: 'tvshows',
        },
      ],
    }
  },
  methods: {
    getDate(dsting) {
      const dobj = new Date(dsting)
      return dobj.toLocaleDateString()
    },
    async addToList() {
      const item = {
        idtvshow: this.tvshow.id,
        nametvshow: this.tvshow.name,
        imgtvshow: this.tvshow.poster_path,
        type: this.tvshowmake.type,
      }

      // 1. ดึงค่า cookie แล้วเช็คว่า มีตัวแปร  tvshow-selected ยัง
      const tvshowSelected = await this.getCookie('tvshow-selected')
      if (tvshowSelected === '') {
        // 1.1ถ้ายังไม่มี tvshow-selected ให้เพิ่มเข้าไป
        const itemJson = JSON.stringify([item])
        await this.setCookie('tvshow-selected', itemJson, 5) // เก็บข้อมูลใน cookie ชื่อ tvshow-selected
        this.makeToast('secondary', item.nametvshow)
        return
      }

      // 1.2 ถ้ามี tvshow-selected แล้วให้เช็คว่า มี หนังที่เลือกอยู่ใน tvshow-selected แล้วยัง
      const tvshowSelectedArr = JSON.parse(tvshowSelected)

      const checkhavetvshowselected = tvshowSelectedArr.some(
        (ts) => ts.idtvshow === item.idtvshow
      )

      // 1.2.1 ถ้ายังไม่มีที่เลือกใน tvshow-selected ให้ push หนังเข้าไป ใน tvshow-selected ได้เลย
      if (checkhavetvshowselected === false) {
        tvshowSelectedArr.push(item)
        const tvshowSelectedArrJson = JSON.stringify(tvshowSelectedArr)
        await this.setCookie('tvshow-selected', tvshowSelectedArrJson, 5)
      }
      this.makeToast('secondary', item.nametvshow)
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
.body-tvshows {
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
  font-size: 22px;
  margin: 0px 5px 0px 5px;
  background-color: rgba(0, 0, 0, 0);
  border: none;
  color: white;
}
.add-button:hover {
  color: rgb(190, 190, 190);
}

.info-tvshows {
  color: rgb(255, 255, 255);
  z-index: 2;
}
.name-tvshows {
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