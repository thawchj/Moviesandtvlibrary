<template>
    <div class="cardbody">
        <nuxt-link v-if="type === 'movie'" :to="`/movies/${id}`"> 
            <div class="my-2">
                <img :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" height="auto" width="100%" >
            </div>
        </nuxt-link> 
        <nuxt-link v-else-if="type === 'tvshow'" :to="`/tvshows/${id}`"> 
            <div class="my-2">
                <img :src="`https://www.themoviedb.org/t/p/w600_and_h900_bestv2${imgpath}`" height="auto" width="100%" >
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
        },
        type: {
            type: String,
            default: ''
        }
    }, 
      
}
</script>

<style scoped>
.cardbody:hover {
    transform: scale(1.1);  
}
.cardbody{
    background-color: rgb(0, 0, 0);
    transition: 0.3s;
}

</style>