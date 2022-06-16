<template>
  <section>
    <button @click="openModal">Edit</button>
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h2 class="gradation">Edit Your Note!</h2>
        <button class="closeButton" @click="closeModal">close</button>
        <form class="row">
          <textarea v-model="EditingText.title" placeholder="Title" class="inputText"></textarea>

          <div class="flexTextarea">
            <div aria-hidden="true">{{ EditingText.description }}</div>
            <textarea v-model="EditingText.description" placeholder="Description" class="inputText"></textarea>
          </div>

          <label for="rating">How many stars?</label>
          <select id="rating" v-model.number="EditingText.rating">
            <option value="5">★★★★★</option>
            <option value="4">★★★★・</option>
            <option value="3">★★★・・</option>
            <option value="2">★★・・・</option>
            <option value="1">★・・・・</option>
          </select>
          <button type="submit" @click.prevent="addCard" class="inputButton unduration pastel">Done !!</button>
        </form>
        <button @click="closeModal">cancel</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
name: 'EditMemo',
props: {
  edittingCard: {},
},
  data(){
    return {
      cabin: {},
      EditingText: {
        title: '',
        description: '',
        rating: '',
        timestamp: '',
        id: ''
      },
      cabinCard: {},
      showContent: false,
    }
  },
  methods: {
    /* 編集後メモオブジェクトをeditedCabinに乗せてemit */
    addCard() {
      this.cabinCard = {
        title: this.EditingText.title,
        description: this.EditingText.description,
        rating: this.EditingText.rating,
        timestamp: new Date(),
        id: this.edittingCard.id,
      }
      this.$emit('editedCabin', this.cabinCard)
      this.clearEditingText()
      this.closeModal()
    },
    /* フォームの初期化 */
    clearEditingText() {
      this.EditingText.title = null
      this.EditingText.description = null
      this.EditingText.rating = null
    },
    /* 編集対象オブジェクトをフォームにセット */
    setCard() {
      this.EditingText.title = this.edittingCard.title
      this.EditingText.description = this.edittingCard.description
      this.EditingText.rating = this.edittingCard.rating
    },
    /* モーダル開閉 */
    openModal() {
      this.showContent = true
      this.setCard()
    },
    closeModal() {
      this.showContent = false
    }
  }
}
</script>
<style scoped>
.content h2 {
  display: block;
  width: 15rem;
  text-align: center;
  margin: 0 auto;
}
textarea,button,select {
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  resize: none;
}
label {
  color: rgb(70, 70, 70);
}
/* モーダルを閉じるボタン */
.closeButton {
  position: absolute;
  top: 0;
  right: 0;
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
</style>