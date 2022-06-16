<template>
<section>
  <div class="row">
    <img alt="logo" src="./assets/logo-note.png">
    <new-memo @memoCabin="getMemo" :cards="notes"></new-memo>
    <h1>{{ info.name }}</h1>
    <div>{{ info.message }}</div>
    <nav>ホーム/設定
      <!-- TODO: naviエリア追加 -->
    </nav>
  </div>
  <div class="row">
    <div>
      <memo-cards :cards="notes" @editedCard="editingCard"></memo-cards>
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
        name: 'MemoApp',
        message: '作成中'
      },
      notes: [
        {
        title: '- Sample Title -',
        description: '- Sample Description -',
        rating: 0,
        timestamp: new Date(),
        id: '0'
        },
      ],
      correctCard: {},
      targetIndex: '',
    }
  },
  methods: {
    /* 表示するメモデータを配列に格納 */
    getMemo(value) {
      this.notes = value
    },
    /* 編集後メモオブジェクトidから対象の[index]を検索しnotesを上書き */
    editingCard(value) {
      this.correctCard = value
      this.targetIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === this.correctCard.id )
      this.notes[this.targetIndex].title = this.correctCard.title
      this.notes[this.targetIndex].description = this.correctCard.description
      this.notes[this.targetIndex].rating = this.correctCard.rating
      this.notes[this.targetIndex].timestamp = this.correctCard.timestamp
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
    margin: 0, 5%;
    font-family: tahoma;
}
.row {
    display: grid;
    vertical-align: middle;
    margin: 2em;
}
/* モーダル */
.overlay {
  z-index: 1;
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
  z-index: 2;
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
/* ボタンインタラクション */
.inputButton {
  position: relative;
  display: inline-block;
  cursor: pointer;
  text-align: center;
  font-weight: 600;
  letter-spacing: 0.1em;
  line-height: 1.5;
  user-select: none;
  transition: all 0.3s;
  margin: 2rem;
  border: none;
}
.unduration {
  font-size: 1.4rem;
  padding: 3rem 4rem;
  color: #fff;
  border-radius: 100% 80px /85px 100%;
}
.unduration:hover {
  color: #fff;
  border-radius: 60% 80px /100% 80%;
}
.pastel {
  background-color: plum;
  background: linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  background: -webkit-linear-gradient(15deg,plum,yellowgreen,lightblue,pink);
  box-shadow: 10px 7px 10px azure;
}
</style>
<style scoped>
img {
  display: inline-block;
}
</style>