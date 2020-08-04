<template>
  <div class="checkin">
    <form class="mt-3">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-6">
            <div class="card bg-light">
              <div class="card-body">
                <h3 class="font-weight-light mb-3">Check in</h3>
                <section class="form-group">
                  <div class="col-12 alert alert-danger px-3" v-if="error">{{error}}</div>
                  <label class="form-control-label sr-only" for="displayName">Name</label>
                  <input
                    required
                    class="form-control"
                    v-model="displayName"
                    type="text"
                    placeholder="Name"
                  />
                </section>
                <section class="form-group">
                  <label class="form-control-label sr-only" for="Email">Email</label>
                  <input
                    required
                    class="form-control"
                    v-model="email"
                    type="email"
                    placeholder="Email"
                  />
                </section>
                <div class="form-group text-right mb-0">
                  <button
                    class="btn btn-primary"
                    @click.prevent="handleCheckin"
                    type="submit"
                  >Check in</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "Checkin",
  data() {
    return {
      displayName: "",
      email: "",
    };
  },
  props: ['error'],
  methods: {
    handleCheckin() {
      this.$emit("checkin", {
        userid: this.$route.params.userid,
        meetingid: this.$route.params.meetingid,
        displayName: this.displayName,
        email: this.email,
      });
      this.displayName = "";
      this.email = "";
    },
  },
};
</script>