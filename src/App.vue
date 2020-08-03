<template>
  <div id="app">
    <Navigation></Navigation>
    <router-view class="container" :user="user" />
  </div>
</template>

<script>
// @ is an alias to /src
import Navigation from "./components/Navigation";
import db from "./db";
//import firebase from "firebase";

export default {
  name: "App",
  data() {
    return {
      user: "",
    };
  },
  components: {
    Navigation,
  },
  mounted() {
    db.collection("users")
      .doc("RqnLWg6MoJab7PFITUU6")
      .get()
      .then((snapshot) => {
        console.log(snapshot);
        console.log(snapshot.data());
        this.user = snapshot.data().name;
      });
  },
};
</script>

<style lang="scss">
$primary: #33cccc;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
