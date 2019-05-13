<template>
  <div id="app">
    <Home v-if="!isLogin"/>
    <Editor v-if="isLogin" :user="userData"/>
  </div>
</template>

<script>
import Home from "./components/Home.vue";
import Editor from "./components/Editor.vue";
import firebase from "firebase/app";
import "firebase/auth";

export default {
  name: 'app',
  components: {
    Home,
    Editor,
  },
  data() {
    return {
      isLogin: false,
      userData: null,
    };
  },
  created() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if (user) {
        this.isLogin = true;
        this.userData = user;
      } else {
        this.isLogin = false;
        this.userData = null;
      }
    });
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
