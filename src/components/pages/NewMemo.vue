<template>
  <section>
    <button class="TopInputButton unduration pastel" @click="openModal">Post new memo!</button>
    <transition name="modal-transition">
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h2 class="gradation">Create Your Note!</h2>
        <button class="closeButton" @click="closeModal">x</button>
        <form class="row">
          <textarea v-model="textInput.title" placeholder="タイトル.." class="inputText"></textarea>

          <div class="flexTextarea">
            <div aria-hidden="true">{{ textInput.description }}</div>
            <textarea v-model="textInput.description" placeholder="説明.." class="inputText"></textarea>
          </div>

          <star-memo ref="starSetting" @starCabin="newStarSet"></star-memo>
          <set-theme ref="themeSetting" @cardsTheme="setNewTheme"></set-theme>

          <div v-show="errorMessage" class="errorMessage">ノートが空白です</div>
          <button type="submit" @click.prevent="addCard" class="inputButton unduration pastel">Done !!</button>
        </form>
        <button class="cancelButton" @click="closeModal">cancel</button>
      </div>
    </div>
    </transition>
  </section>
</template>

<script>
import SetTheme from '../parts/SetTheme.vue'
import StarMemo from '../parts/StarMemo.vue'
export default {
  name: 'NewMemo',
  props: { //新規ID取得getNewId()メソッドで使用
    cards: Array
  },
  data(){
    return {
      textInput: {    //入力テキスト
        title: '',
        description: '',
      },
      rateStars: '',            //レートの受け皿
      themeOfCard: 'Default',   //テーマの受け皿
      cabinCard: {},            //emitする新規メモの受け皿
      showContent: false,       //モーダル開閉
      errorMessage: false,      //空白の場合のエラー表示
    }
  },
  methods: {
    /* 投稿ボタン押下時の操作： 
      ・入力された新規メモオブジェクトをmemoCabinに乗せてemit
      ・モーダルを閉じる
      ・入力情報の初期化 */
    addCard() {
      if (this.textInput.title || this.textInput.description) {
        this.cabinCard = {
          title: this.textInput.title,
          description: this.textInput.description,
          rating: this.rateStars,
          timestamp: new Date(),
          id: this.getNewId(this.cards),
          themeColor: this.themeOfCard
        }
        this.$emit('memoCabin', this.cabinCard )
        this.closeModal()
        this.$emit('goHome')  //GOボタン押下後はHomeタブの投稿後画面に遷移
        this.cabinCard = null
        this.$refs.themeSetting.clearTheme()  //子コンポーネント初期化
      } else {
        this.errorMessage = true    //テキストが空白の場合は投稿禁止とする
        setTimeout(() => {this.errorMessage = false}, 800)
      }
    },
    /* id付与 */
    getNewId(cardsData) {
      if (this.cards.length == 0 ) {
        return 0
      } else {
        let idSerch = cardsData.map((card) => card.id)
        return Math.max(...idSerch) + 1
      }
    },
    /* 子からテーマを取得 */
    setNewTheme(value) {
      this.themeOfCard = value
    },
    /* 子からレートを取得 */
    newStarSet(value) {
      this.rateStars = value
    },
    /* フォーム入力値の初期化 */
    clearTextImput() {
      this.textInput.title = null
      this.textInput.description = null
      this.rateStars = null
      this.themeOfCard = 'Default'
      this.$refs.themeSetting.clearTheme()
      this.$refs.starSetting.clearStars()
    },
    /* モーダル開閉 */
    openModal() {
      this.showContent = true
      this.clearTextImput()
    },
    closeModal() {
      this.showContent = false
      this.clearTextImput()
    }
  },
  components: {
    SetTheme,
    StarMemo,
  }
}
</script>
<style scoped>
/* ボタンインタラクションはHostPageに配置(共通) */
.TopInputButton {
  position: relative;
  display: inline-block;
  width: 19rem;
  cursor: pointer;
  text-align: center;
  font-weight: 600;
  letter-spacing: 0.1em;
  line-height: 3.5;
  user-select: none;
  transition: all 0.3s;
  border: none;
}
.content h2 {
  display: block;
  width: 17rem;
  text-align: center;
  margin: 0 auto;
}
button {
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  resize: none;
}
textarea{
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  resize: none;
  border: 1px solid #464646;
  border-radius: 5px;
}
/* モーダルを閉じるボタン */
.closeButton {
  position: absolute;
  top: 0;
  right: 0;
  box-shadow: none;
}
/* キャンセルボタン */
.cancelButton {
  float: left;
}
/* 文字量に応じて伸縮するテキストボックス */
.flexTextarea {
  position: relative;
  font-size: 1.1rem;
  line-height: 1.5rem;
  margin-bottom: 1rem;
  overflow: hidden;
}
.flexTextarea div {
  overflow: hidden;
  visibility: hidden;
  box-sizing: border-box;
  word-wrap: break-word;
  overflow-wrap: break-word;
  padding: 0.5rem 1rem;
  min-height: 8rem;
  max-height: 13rem;
}
.flexTextarea textarea {
  position: absolute;
  top: 0;
  left: 0;
  display: block;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  font: inherit;
  letter-spacing: inherit;
}
/* 入力テキストスタイル */
.inputText {
  font-size: 1.1rem;
  font-family: tahoma;
}
/* 空白メッセージ */
.errorMessage {
  font-size: 0.8rem;
  color: #ff6347;
}
</style>