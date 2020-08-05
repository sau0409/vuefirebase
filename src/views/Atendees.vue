<template>
  <div class="atendees">
    <div class="container mt-4">
      <div class="row justify-content-center">
        <div
          class="col-8 col-sm-6 col-md-4 col-lg-3 mb-2 p-0 px-1"
          v-for="(atendee, index) in atendees"
          :key="index"
        >
          <div class="card">
            <div class="card-body px-3 py-2 d-flex align-items-center justify-content-center">
              <div class="btn-group pr-2" v-if="user !== null && user.uid === userid">
                <button class="btn btn-small btn-outline-secondary">
                  <FontAwesomeIcon icon="star" class></FontAwesomeIcon>
                </button>
                <a class="btn btn-small btn-outline-secondary">
                  <FontAwesomeIcon icon="envelope" :href="'mailto:'+ atendee.email"></FontAwesomeIcon>
                </a>
                <button class="btn btn-small btn-outline-secondary">
                  <FontAwesomeIcon icon="trash" @click="deleteAtendee(atendee.id) " class></FontAwesomeIcon>
                </button>
              </div>
              <div>{{atendee.displayName}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import db from "../db.js";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

export default {
  name: "Atendees",
  components: {
    FontAwesomeIcon,
  },
  data() {
    return {
      atendees: [],
      userid: this.$route.params.userid,
      meetingid: this.$route.params.meetingid,
    };
  },
  props: ["user"],
  methods: {
    deleteAtendee(id) {
      if (this.user && this.user.uid == this.userid) {
        db
          .collection("users")
          .doc(this.userid)
          .collection("meetings")
          .doc(this.meetingid)
          .collection("atendees")
          .doc(id).delete();
      }
    },
  },
  mounted() {
    db.collection("users")
      .doc(this.userid)
      .collection("meetings")
      .doc(this.meetingid)
      .collection("atendees")
      .onSnapshot((snapshot) => {
        const snapData = [];
        snapshot.forEach((doc) => {
          snapData.push({
            id: doc.id,
            email: doc.data().email,
            displayName: doc.data().displayName,
          });
        });
        this.atendees = snapData;
      });
  },
};
</script>