<template>
  <div class="navigation">
    <nav class="navbar navbar-expand bg-primary navbar-dark">
      <div class="container-fluid">
        <router-link class="navbar-brand" to="/">Meeting Log</router-link>
        <div class="navbar-nav ml-auto">
          <router-link class="nav-item nav-link" to="/">home</router-link>
          <router-link class="nav-item nav-link" to="/meetings" v-if="user">meetings</router-link>
          <router-link class="nav-item nav-link" to="/login" v-if="!user">login</router-link>
          <router-link class="nav-item nav-link" to="/register" v-if="!user">register</router-link>
          <button
            class="nav-item nav-link btn btn-link"
            to="/login"
            v-if="user"
            @click="$emit('logout')"
          >logout</button>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Navigation",
  props: ["user"],
};
</script>