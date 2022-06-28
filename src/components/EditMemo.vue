<template>
  <section>
    <button @click="openModal">Edit</button>
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

          <label for="rating">How many stars?</label>
          <select id="rating" v-model.number="editingText.rating">
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
    </transition>
  </section>
</template>

<script>
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
        title: this.editingText.title,
        description: this.editingText.description,
        rating: this.editingText.rating,
        timestamp: new Date(),
        id: this.editingCard.id,  //IDは元のまま保持する
      }
      this.$emit('editedCabin', this.cabinCard)
      this.cabinCard = null
      this.clearEditingText()
      this.closeModal()
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
textarea,button,select {
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  resize: none;
}
label {
  color: rgb(70, 70, 70);
}
/* モーダルを閉じるボタン配置 */
.closeButton {
  position: absolute;
  top: 0;
  right: 0;
  border-radius: 5px;
  color: rgb(70, 70, 70);
  text-align: center;
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