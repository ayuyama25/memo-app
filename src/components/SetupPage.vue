<template>
  <section>
    <h2 class="gradation">Setup Page:</h2>
    <div class="closeButtonArea">
      <button class="finish" type="submit" @click.prevent="finishConfig">Close</button>
    </div>
      <div class="outlineGroov">
        <div>Default Background Theme :</div>
        <set-theme class="set-theme-div" ref="defaultColorSetting" @cardsTheme="getSetupTheme" :havingTheme="defaultSettings"></set-theme>
        <button class="okButton" type="submit" @click.prevent="giveDefaultTheme">OK !!</button>
      </div>

  </section>
</template>

<script>
import SetTheme from './SetTheme.vue'
export default {
  name: 'SetupPage',
  props: {
    defaultSettings: String
  },
  data() {
    return {
      defaultColorIs: null
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
    /* 初期表示の取得 */
    openConfig() {
      this.$nextTick(this.$refs.defaultColorSetting.removeDefault())
      this.$nextTick(this.$refs.defaultColorSetting.setEditingTheme())
    },
    /* 画面終了しホームタブに戻る */
    finishConfig() {
      this.$emit('backHome')
      this.addDefaulChoice()
      this.defaultColorIs = null
    },
    addDefaulChoice() {
      this.$nextTick(this.$refs.defaultColorSetting.addDefault())
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
  padding: 8%;
  margin: 5%;
  text-align: center;
  border-radius: 10px;
  box-shadow:  -8px 8px 10px #e4e4e4, 8px -8px 10px #fff;
}
.set-theme-div {
  margin: 1.5rem 0;
}
.okButton {
  position: relative;
  box-sizing: border-box;
  width: 70%;
  height: 100%;
  margin: 0 auto;
}
</style>