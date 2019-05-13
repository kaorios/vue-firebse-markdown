<template>
  <div class="editor">
    <h1>Editor</h1>
    <span>{{user.displayName}}</span>
    <button @click="logout">logout</button>
  </div>
</template>

<script>
  import firebase from "firebase/app";
  import "firebase/auth";

  export default {
    name: "Editor",
    props: ["user"],
    data() {
      return {};
    },
    methods: {
      logout: function() {
        firebase.auth().signOut();
      }
    }
  }
</script>
