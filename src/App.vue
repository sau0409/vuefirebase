<template>
  <div id="app">
    <Navigation :user="user" @logout="logout"></Navigation>
    <router-view
      class="container"
      @logout="logout"
      :user="user"
      :meetings="meetings"
      @addMeeting="addMeeting"
      @deletemeeting="deleteMeeting"
      @checkin="checkin"
      :error="error"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import Navigation from "./components/Navigation";
import db from "./db";
import firebase from "firebase";

export default {
  name: "App",
  data() {
    return {
      user: "",
      meetings: [],
      error: ""
    };
  },
  methods: {
    logout() {
      db.app
        .auth()
        .signOut()
        .then(() => {
          this.user = "";
          this.$router.push("/login");
        })
        .catch((err) => (this.error = err.message));
    },
    addMeeting(meetingName) {
      db.collection("users").doc(this.user.uid).collection("meetings").add({
        name: meetingName,
        createdAt: firebase.firestore.FieldValue.serverTimestamp(),
      });
    },
    deleteMeeting(meeting) {
      console.log(meeting);
      db.collection("users")
        .doc(this.user.uid)
        .collection("meetings")
        .doc(meeting.id)
        .delete();
    },
    checkin(obj) {
      console.log(obj);
      db.collection("users").doc(obj.userid).collection("meetings").doc(obj.meetingid)
      .get()
      .then(doc => {
        if(doc.exists) {
              db.collection("users").doc(obj.userid).collection("meetings").doc(obj.meetingid).collection('atendees').add({
                displayName: obj.displayName,
                email: obj.email,
                joinedAt: firebase.firestore.FieldValue.serverTimestamp()
              })
              .then(()=> this.$router.push('/')).catch((err) => console.log(err))
        }
        else {
          console.log("No such meeting.");
          this.error = "No such meeting.";
        }
      })
    },
  },
  components: {
    Navigation,
  },
  mounted() {
    db.app.auth().onAuthStateChanged((user) => {
      if (user) {
        this.user = user;

        db.collection("users")
          .doc(this.user.uid)
          .collection("meetings")
          .onSnapshot((snapshot) => {
            const snapData = [];
            snapshot.forEach((element) => {
              snapData.push({
                id: element.id,
                name: element.data().name,
              });
            });
            this.meetings = snapData.sort((a, b) => {
              const d1 = a.name.toUpperCase();
              const d2 = b.name.toUpperCase();
              let comp = 0;
              if (d1 > d2) {
                comp = 1;
              } else if (d2 > d1) {
                comp = -1;
              }

              return comp;
            });
          });
      }
    });

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
