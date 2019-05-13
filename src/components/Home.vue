<template>
  <div id="home">
    <h1>{{msg}}</h1>
    <button @click="googleLogin">Login (Google Account)</button>
  </div>
</template>

<script>
  import firebase from 'firebase/app';
  import 'firebase/auth';

  export default {
    name: "Home",
    data() {
      return {
        msg: "Welcome to MyMarkdown"
      };
    },
    methods: {
      googleLogin: function() {
        firebase.auth().signInWithRedirect(new firebase.auth.GoogleAuthProvider());
      }
    }
  }
</script>
