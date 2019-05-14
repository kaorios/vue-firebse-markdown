<template>
  <div class="editor">
    <h1>Editor</h1>
    <span>{{user.displayName}}</span>
    <button @click="logout">logout</button>
    <div class="editorWrapper">
      <div class="memoListWrapper">
        <div class="memoList"
             v-for="(memo, index) in memos"
             :key="index"
             @click="selectMemo(index)"
             :data-selected="index === selectedIndex">
          <p class="memoTitle">
            {{displayTitle(memo.markdown)}}
          </p>
        </div>
        <button class="addMemoButton" @click="addMemo">Add Memo</button>
        <button class="deleteMemoButton" v-if="memos.length > '1'" @click="deleteMemo">Delete Memo</button>
        <button class="saveMemoButton" @click="saveMemos">Save Memos</button>
      </div>
      <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
      <div class="preview" v-html="preview()"></div>
    </div>
  </div>
</template>

<script>
  import firebase from "firebase/app";
  import "firebase/auth";
  import "firebase/firestore";
  import marked from "marked";

  export default {
    name: "Editor",
    props: ["user"],
    data() {
      return {
        memos: [
          {
            markdown: "無題のメモ"
          }
        ],
        selectedIndex: 0
      };
    },
    methods: {
      logout: function() {
        firebase.auth().signOut();
      },
      addMemo: function() {
        this.memos.push({
          markdown: "無題のメモ"
        });
      },
      deleteMemo: function() {
        this.memos.splice(this.selectedIndex, 1);
        if (this.selectedIndex > 0) {
          this.selectedIndex--;
        }
      },
      selectMemo: function(index) {
        this.selectedIndex = index;
      },
      saveMemos: function() {
        firebase
            .firestore()
            .collection("memos")
            .doc(this.user.uid)
            .set({ memos: this.memos });
      },
      preview: function() {
        return marked(this.memos[this.selectedIndex].markdown);
      },
      displayTitle: function(text) {
        return text.split(/\n/)[0];
      }
    }
  }
</script>

<style lang="scss" scoped>
  .editorWrapper {
    display: flex;
  }
  .memoListWrapper {
    width: 20%;
    border-top: 1px solid #000;
  }
  .memoList {
    padding: 10px;
    box-sizing: border-box;
    text-align: left;
    border-bottom: 1px solid #000;
    &:nth-child(even) {
      background-color: #ccc;
    }
    &[data-selected="true"] {
      background-color: #ccf;
    }
  }
  .memoTitle {
    height: 1.5em;
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
  }
  .addMemoButton {
    margin-top: 20px;
  }
  .deleteMemoButton {
    margin: 10px;
  }
  .markdown {
    width: 40%;
    height: 500px;
  }
  .preview {
    width: 40%;
    text-align: left;
  }
</style>
