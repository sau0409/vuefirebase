<template>
  <div id="app">
    <Navigation :user="user" @logout="logout"></Navigation>
    <router-view class="container" @logout="logout" :user="user" />
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
  methods: {
    logout() {
      db.app.auth()
      .signOut()
      .then(() => {
        this.user = "";
        this.$router.push('/login');
      }).catch((err) => (this.error = err.message));
    }
  },
  components: {
    Navigation,
  },
  mounted() {
    db.app.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user.displayName;
      }
    })
   /* db.collection("users")
      .doc("RqnLWg6MoJab7PFITUU6")
      .get()
      .then((snapshot) => {
        console.log(snapshot);
        console.log(snapshot.data());
        this.user = snapshot.data().name;
      });*/
  },
};
</script>

<style lang="scss">
$primary: #33cccc;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
