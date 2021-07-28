<template>
  <div class="my-header">
    <b-navbar toggleable="md"  type="dark"  class="my-headerin fixed-top">
      <b-navbar-brand href="#">Movies&TV</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item to="/movies">Movies</b-nav-item>
            <b-nav-item to="/tvshows">TVShows</b-nav-item>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto mr-3">
              <b-nav-item to="/mylist">My List</b-nav-item>
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