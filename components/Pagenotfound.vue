<template>
    <div class=" body">
        <div class="imgbody">
            <div class="textbody">
                <h1 class="show-text">ERROR PAGE NOT FOUND</h1>
                <nuxt-link to="/movies"> 
                    <button class="button-body">BACK</button>
                </nuxt-link>
            </div>
        </div>    
    </div>
</template>
<script>
export default {
    
}
</script>

<style scoped>
.imgbody{
    background-size: cover;
    background-image: linear-gradient(rgba(0, 0, 0, 0.0), rgba(0, 0, 0, 1)),url(https://www.themoviedb.org/t/p/w1920_and_h800_multi_faces/5YBfP6lyEAY0jIk1CYAoQA64q08.jpg);
    background-repeat: no-repeat;
    background-position: center center;
    height: 850px;
    width: 100%;
}
.show-text{
    font-size: 15vw;
    color: white;
    font-weight: bold;
}
.button-body{
    font-size:5vw;
    background: none;
    cursor: pointer;
    border: 0.2vw solid white;
    color: white;
    border-radius: 0.3vw;
    padding: 0.5vw 3vw 0.5vw 3vw;
    font-weight: bold;
}
.textbody{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    padding: 20px 15px;
}
</style>