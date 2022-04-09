<template>
  <div id="app">
    <TitleNav/>
    <OpportunitiesTable />
  </div>
</template>

<script>
import TitleNav from './components/TitleNav.vue'
import OpportunitiesTable from './components/OpportunitiesTable.vue'

export default {
  name: 'App',
  components: {
    TitleNav,
    OpportunitiesTable
  }
}
</script>

<style>
#app {
  font-family:Arial, Helvetica, sans-serif;
  /* font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: black;
  /* color: #2c3e50; */
  /* margin-top: 60px; */
  /* margin-left: 5%; */
}

</style>
