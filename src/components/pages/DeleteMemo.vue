<template>
  <section>
    <button class="deleteButton" @click="openModal">x</button>
    <transition name="modal-transition">
    <div class="overlay" v-show="showContent" @click.self="closeModal">
      <div class="content">
        <h3 class="gradation">{{ title }}</h3>
        <div>{{ message }}</div>
        <button class="options sayYes" @click="deleting"> YES</button>
        <button class="options sayCancel" @click="closeModal">Cancel</button>
      </div>
    </div>
    </transition>
  </section>
</template>

<script>
export default {
  name: 'DeleteMemo',
  props: {
    deletingCardId: {}
  },
  data() {
    return {
      title: 'Would you like to delete this card?',
      message: '削除後の復元はできません。本当に削除しますか？',
      showContent: false,
    }
  },
  methods: {
    deleting() {
      this.closeModal()
      this.$emit('deletingCard', this.deletingCardId)
    },
    /* モーダル開閉 */
    openModal() {
      this.showContent = true
    },
    closeModal() {
      this.showContent = false
    }
  }
}
</script>

<style scoled>
h3 {
  padding: 1rem 0;
}
div {
  text-align: center;
}
/* 削除ボタン本体 */
.deleteButton {
  border-radius: 50%;
  box-shadow: none;
  padding: 0.2rem 0.5rem;
}
.deleteButton:hover {
  box-shadow: none
}
/* モーダル内ボタン Yes/No */
.options {
  margin: 1.25rem;
  padding: 1rem 1.5rem;
  box-sizing: border-box;
  box-shadow:  -3px 3px 10px #dfdfdf, 3px -3px 10px #fff;
}
.sayCancel {
  background: linear-gradient(225deg, #ddd, #fff);
}
.sayCancel:hover {
  background: linear-gradient(225deg, #ddd, #fff);
  box-shadow:  -3px 3px 10px #f3f3f3, 3px -3px 10px #f7f7f7;
}
.sayYes {
  color: #fff;
  background: linear-gradient(225deg, #ff6a4c, #e65940);
}
.sayYes:hover {
  background: linear-gradient(225deg, #e65940, #ff6a4c);
  box-shadow:  -3px 3px 10px #f3f3f3, 3px -3px 10px #f7f7f7;
  color: #fff;
}
</style>