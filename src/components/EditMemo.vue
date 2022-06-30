<template>
  <section>
    <button class="editButton" @click="openModal">Edit</button>
    <transition name="modal-transition">
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h2 class="gradation">Edit Your Note!</h2>
        <button class="closeButton" @click="closeModal">x</button>
        <form class="row">
          <textarea v-model="editingText.title" placeholder="Title" class="inputText"></textarea>
          <div class="flexTextarea">
            <div aria-hidden="true">{{ editingText.description }}</div>
            <textarea v-model="editingText.description" placeholder="Description" class="inputText"></textarea>
          </div>

          <star-memo ref="starEditing" @starCabin="editedStarSet" :havingStar="editingCard.rating"></star-memo>

          <set-theme ref="themeEditing" @cardsTheme="setEditTheme" :havingTheme="editingCard.themeColor"></set-theme>

          <button type="submit" @click.prevent="addCard" class="doneButton inputButton unduration pastel">Done !!</button>
          <div v-show="errorMessage" class="errorMessage">ノートが空白です</div>
        </form>
        <button class="cancelButton" @click="closeModal">cancel</button>
      </div>
    </div>
    </transition>
  </section>
</template>

<script>
import SetTheme from './SetTheme.vue'
import StarMemo from './StarMemo.vue'
export default {
name: 'EditMemo',
props: {
  editingCard: {},
},
  data(){
    return {
      editingText: {
        title: '',
        description: '',
        rating: '',
        timestamp: '',
        themeColor: ''
      },
      editingTheme: '',
      editStars: '',
      cabinCard: {},
      showContent: false,
      errorMessage: false,
    }
  },
  methods: {
    /* 編集後メモオブジェクトをeditedCabinに乗せてemit */
    addCard() {
      if (this.editingText.title || this.editingText.description) {
        this.cabinCard = {
          title: this.editingText.title,
          description: this.editingText.description,
          rating: this.editStars,
          timestamp: this.editingCard.timestamp,  //Dateは元のまま保持する
          id: this.editingCard.id,  //IDは元のまま保持する
          themeColor: this.editingTheme
        }
        this.$emit('editedCabin', this.cabinCard)
        this.cabinCard = null
        this.$refs.themeEditing.clearTheme()
        this.$refs.starEditing.clearStars()
        this.clearEditingText()
        this.closeModal()
      } else {
        this.errorMessage = true
        setTimeout(() => {this.errorMessage = false},500)
      }
    },
    /* フォームの初期化 */
    clearEditingText() {
      this.editingText.title = null
      this.editingText.description = null
      this.editingText.rating = null
    },
    /* 編集対象オブジェクトをフォームにセット */
    setCard() {
      this.editingText.title = this.editingCard.title
      this.editingText.description = this.editingCard.description
      this.editingText.rating = this.editingCard.rating
      this.$refs.themeEditing.setEditingTheme()
      this.$refs.starEditing.setRateStars()
    },
    /* 編集後テーマを回収してセット */
    setEditTheme(value) {
      this.editingTheme = value
    },
    /* レートセット */
    editedStarSet(value) {
      this.editStars = value
    },
    /* モーダル開閉 */
    openModal() {
      this.showContent = true
      this.setCard()
    },
    closeModal() {
      this.showContent = false
      this.clearEditingText()
    }
  },
  components: {
    SetTheme,
    StarMemo
  }
}
</script>
<style scoped>
.content {
  text-align: center;  
}
.content h2 {
  display: block;
  width: 15rem;
  margin: 0 auto;
}
textarea, button {
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  resize: none;
}
/* Editボタン */
  .editButton {
  box-shadow: none;
}
/* モーダルを閉じるボタン配置 */
.closeButton {
  position: absolute;
  top: 0;
  right: 0;
  box-shadow: none;
}
.cancelButton {
  float: left;
}
/* 文字量に応じて伸縮するテキストボックス */
.flexTextarea {
  position: relative;
  font-size: 1.1rem;
  line-height: 1.5;
  margin-bottom: 1rem;
  overflow: hidden;
}
.flexTextarea div {
  overflow: hidden;
  visibility: hidden;
  box-sizing: border-box;
  word-wrap: break-word;
  overflow-wrap: break-word;
  border: solid 1px;
  padding: 0.5rem 1rem;
  min-height: 10rem;
  max-height: 18rem;
}
.flexTextarea textarea {
  position: absolute;
  top: 0;
  left: 0;
  display: block;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  border: solid 1px;
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
  color: tomato;
  text-align: start;
}
</style>