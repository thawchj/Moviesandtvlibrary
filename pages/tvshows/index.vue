<template>
  <div class="body-tvshows">
    <headerlib></headerlib>
    <b-carousel
      id="carousel-fade"
      style="text-shadow: 0px 0px 2px #000"
      fade
      indicators
      img-width="1024"
      img-height="480"
      class="d-none d-md-block"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <div v-if="tvshowsArr && tvshowsArr.length > 0">
        <div v-for="(n, index) in 5" :key="index">
          <b-carousel-slide>
            <template #img>
              <img
                class="d-block img-fluid w-100"
                width="100%"
                :src="`https://www.themoviedb.org/t/p/w1920_and_h800_multi_faces/${tvshowsArr[index].backdrop_path}`"
                alt="image slot"
              />
              <div class="carousel-in">
                <div class="row">
                  <div class="col-8">
                    <h1 class="carousel-name">{{ tvshowsArr[index].name }}</h1>
                  </div>
                </div>
                <div class="my-3">
                  <nuxt-link :to="`/tvshows/${tvshowsArr[index].id}`">
                    <button class="carousel-button">More Info</button>
                  </nuxt-link>
                </div>
              </div>
            </template>
          </b-carousel-slide>
        </div>
      </div>
    </b-carousel>
    <div class="type">TvShows</div>
    <div class="container-fluid px-5">
      <div class="row">
        <div
          v-for="m in tvshowsArr"
          :key="m.id"
          class="col-6 col-sm-4 col-md-3 col-lg-2"
        >
          <cardlib :id="m.id" :name="m.name" :imgpath="m.poster_path"></cardlib>
        </div>
      </div>
    </div>
    <div class="text-center">
      <button type="button" class="buttonload" @click="loadmore()">
        LOAD MORE
      </button>
    </div>
    <footerlib></footerlib>
  </div>
</template>


<script>
import Cardlib from '~/components/Cardlib.vue'
import Footerlib from '~/components/Footerlib.vue'
import Headerlib from '~/components/Headerlib.vue'
export default {
  components: { Cardlib, Footerlib, Headerlib },
  async asyncData({ $axios }) {
    const res = await $axios.$get(
      `https://api.themoviedb.org/3/discover/tv?api_key=${process.env.API_KEY}&query=a&sort_by=popularity.desc&vote_count.gte=200&page=1`
    )
    return {
      tvshowsArr: res.results,
    }
  },
  data() {
    return {
      tvshowsArr: null,
      slide: 0,
      sliding: null,
      count: 2,
    }
  },
  head() {
    return {
      title: 'tvshows',
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
    onSlideStart(slide) {
      this.sliding = true
    },
    onSlideEnd(slide) {
      this.sliding = false
    },
    async loadmore() {
      const res = await this.$axios.$get(
        `https://api.themoviedb.org/3/discover/tv?api_key=${process.env.API_KEY}&query=a&sort_by=popularity.desc&vote_count.gte=200&page=${this.count}`
      )
      if (res && res.results && res.results.length > 0) {
        this.tvshowsArr = [...this.tvshowsArr, ...res.results]
        this.count++
      }
    },
  },
}
</script>

<style scoped>
.body-tvshows {
  background-color: black;
}
.carousel-in {
  position: absolute;
  top: 25%;
  left: 10%;
  text-align: start;
  color: rgb(255, 255, 255);
}
.carousel-name {
  font-size: 6vw;
  font-weight: bold;
}
.carousel-button {
  font-size: 1.3vw;
  background: none;
  cursor: pointer;
  border: 0.2vw solid white;
  color: white;
  border-radius: 0.3vw;
  padding: 0.5vw 1.2vw 0.5vw 1.2vw;
  font-weight: bold;
}
.carousel-button:hover {
  border-color: rgb(231, 230, 230);
  color: rgb(231, 230, 230);
}
.type {
  font-size: 45px;
  color: white;
  margin: 40px 0px 20px 20px;
  font-weight: bold;
}
.buttonload {
  padding: 4px 80px;
  border: 2px solid white;
  background-color: Transparent;
  font-size: 20px;
  color: white;
  font-weight: bolder;
  margin-top: 20px;
}
.buttonload:hover {
  color: rgb(190, 190, 190);
  border: 2px solid rgb(190, 190, 190);
}
</style>