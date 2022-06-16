<template>
  <section>
    <button class="inputButton unduration pastel" @click="openModal">Create new note!</button>
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h2 class="gradation">Create Your Note!</h2>
        <button class="closeButton" @click="closeModal">close</button>
        <form class="row">
          <textarea v-model="textInput.title" placeholder="Title.." class="inputText"></textarea>

          <div class="flexTextarea">
            <div aria-hidden="true">{{ textInput.description }}</div>
            <textarea v-model="textInput.description" placeholder="Description.." class="inputText"></textarea>
          </div>

          <label for="rating">How many stars?</label>
          <select id="rating" v-model.number="textInput.rating">
            <option value="5">★★★★★</option>
            <option value="4">★★★★・</option>
            <option value="3">★★★・・</option>
            <option value="2">★★・・・</option>
            <option value="1">★・・・・</option>
          </select>

          <button type="submit" @click.prevent="addCard" class="inputButton unduration pastel">GO !!</button>
        </form>
        <button @click="closeModal">cancel</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'NewMemo',
  props: {
    cards: Array
  },
  data(){
    return {
      cabin: [],
      textInput: {
        title: '',
        description: '',
        rating: '',
        timestamp: '',
        id: '',
      },
      cabinCard: {},
      showContent: false,
      idSerch: new Map()
    }
  },
  methods: {
    /* 新規メモオブジェクトをmemoCabinに乗せてemit */
    addCard() {
      if (this.textInput.title || this.textInput.description) {
        this.cabinCard = {
          title: this.textInput.title,
          description: this.textInput.description,
          rating: this.textInput.rating,
          timestamp: new Date(),
          id: this.getNewId(this.cards)
        }
        this.cabin.unshift(this.cabinCard)
        this.$emit('memoCabin', this.cabin)
        this.clearTextImput()
        this.closeModal()
      }
    },
    /* id付与 */
    getNewId(cardsData) {
      this.idSerch = cardsData.map((card) => card.id)
      return Math.max(...this.idSerch) + 1
    },
    /* テキストボックス入力値の初期化 */
    clearTextImput() {
      this.textInput.title = null
      this.textInput.description = null
      this.textInput.rating = null
      this.textInput.timestamp = null
    },
    /* モーダル開閉 */
    openModal() {
      this.showContent = true
    },
    closeModal() {
      this.showContent = false
      this.clearTextImput()
    }
  }
}
</script>
<style scoped>
.content h2 {
  display: block;
  width: 17rem;
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