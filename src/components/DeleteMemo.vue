<template>
  <section>
    <button class="deleteButton" @click="openModal">x</button>
    <transition name="modal-transition">
    <div class="overlay" v-show="showContent" @click.self="closeModal">
      <div class="content">
        <h3 class="gradation">{{ title }}</h3>
        <div>{{ message }}</div>
        <button class="options sayYes" @click="deleting"> YES</button>
        <button class="options" @click="closeModal">Cancel</button>
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
      message: 'Once the note is deleted, it cannot be restored.',
      showContent: false,
    }
  },
  methods: {
    deleting() {
      this.closeModal()
      this.$nextTick(this.$emit('deletingCard', this.deletingCardId))
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
/* 削除ボタン本体 */
.deleteButton {
  border-radius: 50%;
  color: rgb(70, 70, 70);
  text-align: center;
  opacity: 0.9;
}
/* モーダル内ボタン */
.options {
  margin: 1.25rem;
  padding: 1rem 1.5rem;
  box-sizing: border-box;
}
/* .options:hover {
  transition: all 0.1s ease-out;
  outline: 2px solid #fff;
  outline-offset: -1px;
} */
.sayYes {
  background-color: tomato;
  color: #fff;
}
</style>