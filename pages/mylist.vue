<template>
  <div class="body">
    <div class="body-mylist">
      <headerlib></headerlib>
      <div class="container">
        <div class="d-flex justify-content-between">
          <p class="mylist">Mylist</p>
          <button class="button-clear" @click="clear()">clear</button>
        </div>
        <p class="mylist-type">Movie</p>
        <hr class="line" />
        <div class="row">
          <cardlibselected
            v-for="ms in movieselected"
            :id="ms.idmovie"
            :key="ms.id"
            :name="ms.namemovie"
            :imgpath="ms.imgmovie"
            :type="ms.type"
            class="col-4 col-md-3 col-lg-2"
          >
          </cardlibselected>
        </div>
        <p class="mylist-type">Tvshow</p>
        <hr class="line" />
        <div class="row">
          <cardlibselected
            v-for="ss in tvshowselected"
            :id="ss.idtvshow"
            :key="ss.id"
            :name="ss.nametvshow"
            :imgpath="ss.imgtvshow"
            :type="ss.type"
            class="col-4 col-md-3 col-lg-2"
          >
          </cardlibselected>
        </div>
      </div>
    </div>
    <footerlib></footerlib>
  </div>
</template>

<script>
import Cardlibselected from '~/components/Cardlibselected.vue'
import Footerlib from '~/components/Footerlib.vue'
import Headerlib from '~/components/Headerlib.vue'
export default {
  components: { Cardlibselected, Footerlib, Headerlib },
  props: {
    idmovie: {
      type: Number,
      default: 0,
    },
    namemovie: {
      type: String,
      default: '',
    },
    imgmovie: {
      type: String,
      default: '',
    },
    idtvshow: {
      type: Number,
      default: 0,
    },
    nametvshow: {
      type: String,
      default: '',
    },
    imgtvshow: {
      type: String,
      default: '',
    },
    type: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      movieselected: [],
      tvshowselected: [],
    }
  },
  mounted() {
    if (this.getCookie('movie-selected') !== '') {
      this.movieselected = JSON.parse(this.getCookie('movie-selected'))
    }
    if (this.getCookie('tvshow-selected') !== '') {
      this.tvshowselected = JSON.parse(this.getCookie('tvshow-selected'))
    }
  },
  methods: {
    clear() {
      const cleardatatvshow = this.getCookie('tvshow-selected')
      const cleardatamovie = this.getCookie('movie-selected')
      if (cleardatatvshow !== '') {
        this.setCookie('tvshow-selected', '', 5)
      }
      if (cleardatamovie !== '') {
        this.setCookie('movie-selected', '', 5)
      }
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
.body-mylist {
  min-height: 800px;
}
.mylist {
  color: white;
  font-size: 50px;
  padding-top: 70px;
  font-weight: bold;
}
.mylist-type {
  font-size: 28px;
  color: white;
  margin-top: 20px;
  font-weight: bold;
}
.line {
  border-top: 1px solid rgb(94, 93, 93);
}
.button-clear {
  padding: 0px 40px;
  border: 2px solid white;
  background-color: Transparent;
  font-size: 20px;
  color: white;
  font-weight: bolder;
  margin-top: 90px;
  height: 50px;
}
.button-clear:hover {
  color: rgb(190, 190, 190);
  border: 2px solid rgb(190, 190, 190);
}
</style>