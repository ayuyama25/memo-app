<template>
<section>
  <div class="back-ground-string-div">
      <back-ground-string></back-ground-string>
  </div>

  <div class="row">
    <img class="logoNOTE" alt="logo" src="./assets/logo-note.png">
    <div class="new-memo-div">
      <new-memo @memoCabin="getMemo" :cards="notes" @goHome="tabChangeHome"></new-memo>
    </div>
    <h1>{{ info.name }}</h1>
    <div>{{ info.message }}</div>
  </div>
  
  <nav>
    <li @click="tabChangeHome" :class="{'pushed': homeTab}">ホーム</li>
    <li @click="tabChangeSetup" :class="{'pushed': !homeTab}">設定</li>
  </nav>
  <span class="stepLine"></span>
  <div class="row" v-show="homeTab">
    <div class="memo-cards-div">
      <memo-cards :cards="notes" :optionTheme="defaultColor" @editedCard="editingCard" @deletedId="getDeleted"></memo-cards>
    </div>
  </div>

  <div class="row" v-show="!homeTab">
    <div class="setup-page-div">
      <setup-page ref="configPage" @setupColor="getDefaultColor" @backHome="tabChangeHome" :defaultSettings="defaultColor"></setup-page>
    </div>
  </div>
</section>
</template>
<script>
import MemoCards from './components/pages/MemoCards.vue'
import NewMemo from './components/pages/NewMemo.vue'
import SetupPage from './components/pages/SetupPage.vue'
import BackGroundString from './components/parts/BackGroundString.vue'
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
        title: '- Sample : オリジナルのメモを作成しよう！ -',
        description: '- 「Post new memo!」ボタンを押して新規メモを作成する。 -',
        rating: 0,
        timestamp: new Date(),
        id: '0',
        themeColor: 'Default',
        },
      ],
      homeTab: true,
      defaultColor: 'cotton',
    }
  },
  methods: {
    /* 新規メモデータをnotes配列に格納 */
    getMemo(value) {
      this.notes.push(value)
    },
    /* 編集後メモオブジェクトidから対象の[index]を検索しnotesを上書き */
    editingCard(value) {
      let targetIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === value.id )
      this.notes[targetIndex].title = value.title
      this.notes[targetIndex].description = value.description
      this.notes[targetIndex].rating = value.rating
      this.notes[targetIndex].timestamp = value.timestamp
      this.notes[targetIndex].themeColor = value.themeColor
      return this.notes
    },
    /* 削除対象idからnotesの[index]を検索して削除実行 */
    getDeleted(value) {
      let deleteIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === value )
      this.notes.splice([deleteIndex],1)
      return this.notes
    },
    /* 子からデフォルトテーマを取得 */
    getDefaultColor(value) {
      this.defaultColor = value
    },
    /* ナビゲーションタブ切り替え */
    tabChangeHome() {
      this.homeTab = true
      this.$refs.configPage.addDefaulChoice()
    },
    tabChangeSetup() {
      this.$refs.configPage.openConfig()
      this.homeTab = false
    },
  },
  components: {
    MemoCards,
    NewMemo,
    SetupPage,
    BackGroundString,
  }
}
</script>
<style>
body {
  background-color: #f5f5f5;
  margin: 0 5%;
  font-family: tahoma;
  text-align: center;
  color: #464646
}
button {
  border: none;
  padding: 1rem 1.5rem;
  font-weight: bold;
  color: #787878;
  border-radius: 5px;
  transition: all 0.2s ease-out;
  background-color: linear-gradient(225deg, #ddd, #fff);
  box-shadow:  -3px 3px 10px #dfdfdf, 3px -3px 10px #fff;
}
button:hover {
  color: #464646;
  cursor: pointer;
  background-color: linear-gradient(225deg, #fff, #ddd);
  box-shadow: inset -3px 3px 15px #dfdfdf, inset 3px -3px 15px #fff;
}
input:hover, label:hover {
  cursor: pointer;
}
h2 {
  padding-top: 1rem;
}
.row {
  display: grid;
  vertical-align: middle;
  margin: 1rem;
}
.logoNOTE {
  z-index: 1;
}
.new-memo-div {
  padding: 7% 0 0 0;
}
.memo-cards-div {
  text-align: start;
}
.setup-page-div {
  text-align: start;
}
.back-ground-string-div {
  position: absolute;
  top: 0;
  left: 0;
}
/* ナビゲーションタブ */
nav {
  display: flex;
  padding: 0 15%;
}
nav li {
  list-style: none;
  padding: 0.5rem 1rem;
  width: 50%;
  border-radius: 20px 20px 0 0;
  background: linear-gradient(225deg, #ddd, #fff);
  box-shadow:  -10px 10px 20px #d8d8d8, 10px -10px 20px #fff;
  color: #787878;
  -webkit-text-stroke: 0.1px #f0ffff;
}
nav li:first-child {
  margin-right: 0.5rem;
}
nav li:hover {
  background: linear-gradient(225deg, #fff, #d8e6e6);
  box-shadow: inset -8px 8px 15px #dae8e8, inset 8px -8px 15px #fff;
  cursor: pointer;
  color: #464646;
  transition: 0.3s ease-out;
}
.pushed {
  background: linear-gradient(225deg, #fff, #ddd);
  box-shadow: inset -8px 8px 15px #dae8e8, inset 8px -8px 15px #fff;
  transition: 0.3s ease-out;
  color: #787878;
  text-shadow: 0 0 2px #fff, 0 0 0.5rem #f0ffff, 0 0 1.5rem #f0ffff;
}
.stepLine {
  margin: 0;
  padding: 0;
  display: block;
  position: absolute;
  left: 0;
  width: 100vw;
  border: 11px solid #f5f5f5;
  box-shadow:  0 8px 10px #f5f5f5, 0 -5px 15px #f0ffff;
}
/* モーダル */
.overlay {
  z-index: 10;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
}
.content {
  z-index: 100;
  width: 75%;
  padding: 1rem;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: -2rem 2rem 3rem -6px rgba(0, 0, 0, 0.3);
}
.modal-transition-leave-active, .modal-transition-enter-active {
  opacity: 0;
  transition: 0.3s ease;
}
/* 文字グラデーション */
.gradation {
  display: inline-block;
  background: linear-gradient(15deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  background: -webkit-linear-gradient(15deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
/* グラデーションボタン -共通 */
.inputButton {
  position: relative;
  display: block;
  margin-top: 0.5rem;
  cursor: pointer;
  text-align: center;
  text-shadow: 0 0 1px #464646;
  font-weight: 600;
  letter-spacing: 0.1em;
  user-select: none;
  transition: all 0.3s ease-out;
  border: none;
}
.unduration {
  font-size: 1.4rem;
  padding: 4rem;
  color: #fff;
  border-radius: 100% 80px /85px 100%;
  text-shadow: none;
}
.unduration:hover {
  color: #f0ffff;
  border-radius: 60% 150px /100% 85px;
  text-shadow: 0 0 1px #fff;
}
.pastel {
  background-color: #de7dde;
  background: linear-gradient(15deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  background: -webkit-linear-gradient(15deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  box-shadow: 5px 5px 10px #d8d8d8, -5px -5px 10px #f0ffff;
}
.pastel:hover {
  background: linear-gradient(195deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  background: -webkit-linear-gradient(195deg,#f088f0,#c8ed7d,#81d0ea,#ea8697);
  box-shadow: 5px 5px 10px #d8d8d8, -5px -5px 10px #f0ffff;  
}
</style>