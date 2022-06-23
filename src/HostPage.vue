<template>
<section>
  <div class="row">
    <img alt="logo" src="./assets/logo-note.png">
    <div class="new-memo-div">
      <new-memo @memoCabin="getMemo" :cards="notes"></new-memo>
    </div>
    <h1>{{ info.name }}</h1>
    <div>{{ info.message }}</div>
    <nav>ホーム/設定
      <!-- TODO: naviエリア追加 -->
    </nav>
  </div>
  <div class="row">
    <div class="memo-cards-div">
      <memo-cards :cards="notes" @editedCard="editingCard" @deletedId="getDeleted"></memo-cards>
    </div>
  </div>
</section>
</template>
<script>
import MemoCards from './components/MemoCards.vue'
import NewMemo from './components/NewMemo.vue'
export default {
  name: 'HostPage',
  data() {
    return {
      info: {
        name: '↑ Click !!',
        message: ''  //メッセージ欄
      },
      notes: [
        {
        title: '- Sample : Create your original note! -',
        description: '-  Press the "Post new memo!" to start -',
        rating: 0,
        timestamp: new Date(),
        id: '0'
        },
      ],
      newNotes: {},
      changeCard: {},
      targetIndex: '',
    }
  },
  methods: {
    /* 新規メモデータをnotes配列に格納 */
    getMemo(value) {
      this.newNotes = value
      this.notes.push(this.newNotes)
      return this.newNotes = null
    },
    /* 編集後メモオブジェクトidから対象の[index]を検索しnotesを上書き */
    editingCard(value) {
      this.changeCard = value
      this.targetIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === this.changeCard.id )
      this.notes[this.targetIndex].title = this.changeCard.title
      this.notes[this.targetIndex].description = this.changeCard.description
      this.notes[this.targetIndex].rating = this.changeCard.rating
      this.notes[this.targetIndex].timestamp = this.changeCard.timestamp
      this.targetIndex = null
      this.changeCard = null
      return this.notes
    },
    /* 削除対象idからnotesの[index]を検索して削除実行 */
    getDeleted(value) {
      this.changeCard = value
      this.targetIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === this.changeCard )
      this.notes.splice([this.targetIndex],1)
      this.targetIndex = null
      this.changeCard = null
      return this.notes
    }
  },
  components: {
    MemoCards,
    NewMemo,
  }
}
</script>
<style>
body {
  background-color: #f2f2f2;
  margin: 0 5%;
  font-family: tahoma;
  text-align: center;
  color: rgb(70, 70, 70)
}
button {
  border: none;
  font-weight: bold;
  color: rgb(70, 70, 70)
}
h2 {
  padding-top: 1rem;
}
.row {
  display: grid;
  vertical-align: middle;
  margin: 2em;
}
.new-memo-div {
  padding: 7% 0 0 0;
}
.memo-cards-div {
  text-align: start;
}
/* モーダル */
.overlay {
  z-index: 10;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
.content {
  z-index: 100;
  width: 75%;
  padding: 1rem;
  background: #fff;
}
/* 文字グラデーション */
.gradation {
  display: inline-block;
  background: linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  background: -webkit-linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
/* グラデーションボタンインタラクション -共通 */
.inputButton {
  position: relative;
  display: inline-block;
  margin-top: 1rem;
  cursor: pointer;
  text-align: center;
  font-weight: 600;
  letter-spacing: 0.1em;
  line-height: 1.5;
  user-select: none;
  transition: all 0.3s;
  border: none;
}
.unduration {
  font-size: 1.4rem;
  padding: 4rem;
  color: #fff;
  border-radius: 100% 80px /85px 100%;
}
.unduration:hover {
  font-size: 1.41rem;
  color: azure;
  border-radius: 60% 100px /100% 85%;
}
.pastel {
  background-color: plum;
  background: linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  background: -webkit-linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  box-shadow: 10px 7px 10px azure;
}
</style>