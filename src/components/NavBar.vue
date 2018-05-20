<template>
  <nav class="navbar is-transparent">
    <div class="navbar-brand">
        <router-link to="/" class="navbar-item">Coconow</router-link>
      <div @click="showDropDown = !showDropDown" class="navbar-burger burger" data-target="navbarExampleTransparentExample">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>

    <div id="navbarExampleTransparentExample" class="navbar-menu" :class="{ 'is-active': showDropDown}">
      <div class="navbar-start">
        <router-link to="/" class="navbar-item">
          Home
        </router-link>
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <template v-if="currentUser">
            <p>{{ currentUser.displayName }}</p>
            <p><img :src="currentUser.photoURL" alt=""></p>
            <a @click="logout" class="navbar-item">Logout</a>
          </template>
          <template v-else>
            <router-link v-if="!currentUser" to="/login" class="navbar-item">Login</router-link>
          </template>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import firebase from 'firebase'
export default {
  name: 'nav-bar',
  data () {
    return {
      currentUser: null,
      showDropDown: false
    }
  },
  created () {
    firebase.auth().onAuthStateChanged((user) => {
      if (!user) {
        this.currentUser = null
      } else {
        this.currentUser = user
        console.log(user)
      }
    });
  },
  methods: {
    logout () {
      firebase.auth().signOut().then(() => {
        this.currentUser = null
        this.$router.push("/login")
      })
    }
  }
}
</script>
