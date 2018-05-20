<template>
  <div>
    <div v-if="userData.name.length >0">
      <p><img :src="userData.image" alt=""></p>
      <p>{{ userData.name }}がログインしました。</p>
    </div>
    <button @click="loginWithTwitter">Login with twitter</button>
    <button @click="loginWithGoogle">Login with google</button>
  </div>
</template>

<script>
import db from './firebaseInit'
import firebase from 'firebase'
export default {
  name: 'login',
  data () {
    return {
    }
  },
  created () {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.$router.push('/')
      }
    })
  },
  methods: {
    loginWithTwitter () {
      this.login(new firebase.auth.TwitterAuthProvider())
    },
    loginWithGoogle () {
      this.login(new firebase.auth.GoogleAuthProvider())
    },
    login (provider) {
      firebase.auth().signInWithPopup(provider).then((result) => {
        this.$router.push('/')
      })
      .catch((error) => {
        console.log(error)
      })
    }
  }
}
</script>

<style scoped>
</style>
