<template>
  <div class="navigation">
    <nav class="navbar navbar-expand bg-primary navbar-dark">
      <div class="container-fluid">
        <router-link class="navbar-brand" to="/">Meeting Log</router-link>
        <div class="navbar-nav ml-auto">
          <router-link class="nav-item nav-link" to="/">home</router-link>
          <router-link class="nav-item nav-link" to="/meetings">meetings</router-link>
          <router-link class="nav-item nav-link" to="/login">login</router-link>
          <router-link class="nav-item nav-link" to="/register">register</router-link>
        </div>
      </div>
    </nav>
  </div>
</template>