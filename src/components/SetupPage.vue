<template>
  <section>
    <h2 class="gradation">Setup Page:</h2>
    <div class="closeButtonArea">
      <button class="finish" type="submit" @click.prevent="finishConfig">Close</button>
    </div>
      <div class="outlineGroov">
        <div>Default Background Theme :</div>
        <set-theme @cardsTheme="getSetupTheme" :derfaultTheme="defaultSetupTheme"></set-theme>
        <button type="submit" @click.prevent="giveDefaultTheme">OK !!</button>
      </div>

  </section>
</template>

<script>
import SetTheme from './SetTheme.vue'
export default {
  name: 'SetupPage',
  data() {
    return {
      defaultColorIs: 'happiness'
    }
  },
  methods: {
    /* コンポーネントから設定結果を取得 */
    getSetupTheme(value) {
      this.defaultColorIs = value
    },
    /* 親に結果をemit、初期化してタブを閉じる */
    giveDefaultTheme() {
      this.$emit('setupColor', this.defaultColorIs)
      this.finishConfig()
    },
    finishConfig() {
      this.$emit('backHome')
      this.defaultColorIs = null
    }
  },
  components: {
    SetTheme,
  }
}
</script>

<style scoped>
.closeButtonArea {
  position: relative;
  text-align: right;
}
.outlineGroov {
  padding: 5%;
  margin: 5%;
  border-radius: 10px;
  box-shadow:  -8px 8px 10px #e4e4e4, 8px -8px 10px #ffffff;
}
</style>