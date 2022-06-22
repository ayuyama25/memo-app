<template>
  <section>
    <button @click="openModal">Delete</button>
    <div class="overlay" v-show="showContent">
      <div class="content">
        <h3 class="gradation">{{ title }}</h3>
        <div>{{ message }}</div>
        <button @click="deleting"> OK</button>
        <button @click="closeModal">Cancel</button>
      </div>
    </div>
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
      title: 'Would you like to delete?',
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

</style>