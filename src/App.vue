<template>
  <div id="app">
    <Navigation></Navigation>
    <router-view class="container"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Navigation from "./components/Navigation";

export default {
  name: 'App',
  components: {
    Navigation
  }
}
</script>

<style lang="scss">
$primary: #33cccc;
@import "node_modules/bootstrap/scss/bootstrap"
</style>
