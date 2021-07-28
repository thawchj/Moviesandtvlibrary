<template>
  <div class="my-header">
    <b-navbar toggleable="md"  type="dark"  class="my-headerin fixed-top">
      <b-navbar-brand href="#">Movies&TV</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <nuxt-link class="link" to="/movies">
              <b-nav-item class="headermovies" >Movies</b-nav-item>
            </nuxt-link >
            <nuxt-link class="link" to="/tvshows">
              <b-nav-item >TVShows</b-nav-item>
            </nuxt-link>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto mr-3">
            <nuxt-link class="link" to="/mylist">
              <b-nav-item >My List</b-nav-item>
            </nuxt-link>
          </b-navbar-nav>
        </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.my-headerin {
  width: 100%;
  padding: 5px;
  background: rgb(2,0,36);
  background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(24,32,34,1) 0%, rgba(25,25,25,0.5382528011204482) 100%, rgba(0,212,255,1) 100%);
}
.link:hover{
   text-decoration: none;
}

</style>