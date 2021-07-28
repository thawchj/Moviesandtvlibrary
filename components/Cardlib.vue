<template>
    <div class="cardbody mb-4">
        <nuxt-link v-if="currentPage === 'movies'" target="_blank" :to="`/movies/${id}`">
            <div v-b-hover="handleHover" class="img-container" >
                <div v-if="isHovered">
                    <img class="card-img" :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" alt="" height="auto" width="100%">
                    <div class="card-in">
                        <p class="card-in-name m-0" >{{name}}</p>
                    </div>
                    <div class="card-black-cover"></div>
                </div>
                <img v-else class="card-img" :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" alt="" height="auto" width="100%">  
            </div>
        </nuxt-link>
        <nuxt-link v-else-if="currentPage === 'tvshows'" target="_blank" :to="`/tvshows/${id}`">
            <div v-b-hover="handleHover" class="img-container" >
                <div v-if="isHovered">
                    <img class="card-img" :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" alt="" height="auto" width="100%">
                    <div class="card-in">
                        <p class="card-in-name m-0" >{{name}}</p>
                    </div>
                    <div class="card-black-cover"></div>
                </div>
                <img v-else class="card-img" :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" alt="" height="auto" width="100%">  
            </div>
        </nuxt-link>
    </div>
</template>

<script>
export default {
    props: {
        id: {
            type: Number,
            default: 0
        },
        name: {
            type: String,
            default: ''
        },
        imgpath: {
            type: String,
            default: ''
        }
    },
    data() {
      return {
        isHovered: false,
        currentPage: this.$route.name
      }
    },
    methods: {
      handleHover(hovered) {
        this.isHovered = hovered
      }
    }
}
</script>

<style scoped>
.img-container{
    position: relative;
    
}
.card-in-name{
    font-size: 18px;
    font-weight: bold;
    color: white;
    line-height: 80%;
}

.card-in{
    position: absolute;
    z-index: 2;
    top: 85%;
    left: 2%; 
    border-style:none;
}
.card-black-cover{
    background: rgb(18,18,18);
    background: linear-gradient(rgba(0, 0, 0, 0.0), rgba(0, 0, 0, 1));
    position: absolute;
    width: 100%;
    height: 25%;
    bottom: 0;
    left: 0;
    z-index: 1;   
}
.cardbody:hover {
    transform: scale(1.1);
    
}
.cardbody{
    background-color: rgb(0, 0, 0);
    transition: 0.3s;
}
.card-img{
    border-radius: 3px;
}
</style>