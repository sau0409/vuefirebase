<template>
  <div class="login">
    <div>
      <form class="mt-3" @submit.prevent="login">
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-lg-6">
              <div class="card bg-light">
                <div class="card-body">
                  <h3 class="font-weight-light mb-3">Log in</h3>
                  <section class="form-group">
                    <div v-if="error" class="col-12 alert alert-danger px-3">{{error}}</div>
                    <label class="form-control-label sr-only" for="Email">Email</label>
                    <input
                      required
                      class="form-control"
                      type="email"
                      id="email"
                      v-model="email"
                      placeholder="Email"
                    />
                  </section>
                  <section class="form-group">
                    <input
                      required
                      class="form-control"
                      type="password"
                      v-model="password"
                      placeholder="Password"
                    />
                  </section>
                  <div class="form-group text-right mb-0">
                    <button class="btn btn-primary" type="submit">Log in</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </form>
      <p class="text-center mt-2">
        or
        <router-link to="/register">register</router-link>
      </p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import firebase from "firebase";

export default {
  name: "Login",
  components: {},
  data() {
    return {
      email: "",
      password: "",
      error: "",
    };
  },
  methods: {
    login() {
      const info = {
        email: this.email,
        password: this.password,
      };
      firebase
        .auth()
        .signInWithEmailAndPassword(info.email, info.password)
        .then(
          () => {
            this.$router.push("meetings");
          }
        ).catch((err) => (this.error = err.message))
    },
  },
};
</script>
