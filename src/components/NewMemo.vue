<template>
  <section>
    <button class="TopInputButton unduration pastel" @click="openModal">Post new memo!</button>
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h2 class="gradation">Create Your Note!</h2>
        <button class="closeButton" @click="closeModal">x</button>
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
          <div v-show="errorMessage" class="errorMessage">ノートが空白です</div>
        </form>
        <button @click="closeModal">cancel</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'NewMemo',
  props: { //新規ID取得getNewId()メソッドで使用
    cards: Array
  },
  data(){
    return {
      textInput: {
        title: '',
        description: '',
        rating: '',
        timestamp: '',
        id: '',
      },
      cabinCard: {},
      showContent: false,
      errorMessage: false,
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
        this.$emit('memoCabin', this.cabinCard )
        this.closeModal()        
        this.idSerch = null
        this.cabinCard = null
      } else {
        this.errorMessage = true
        setTimeout(() => {
          this.errorMessage = false }
          ,500
        )
      }
    },
    /* id付与 */
    getNewId(cardsData) {
      if (this.cards.length == 0 ) {
        return 0
      } else {
        this.idSerch = cardsData.map((card) => card.id)
        return Math.max(...this.idSerch) + 1
      }
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
/* ボタンインタラクションはHostPageに配置、全ボタン共通 */
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
/* 空白メッセージ */
.errorMessage {
  font-size: 0.8rem;
  color: tomato;
  text-align: start;
}
</style>