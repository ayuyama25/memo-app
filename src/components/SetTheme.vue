<template>
  <section>
  <div class="chosen-theme">
    <span>
      <div class="inputs">
        <div v-for="(value, index) in this.values" :key="index" @change="choseTheme">
        <input type="radio" :id="value.index" :value="value" v-model="theme">
        </div>
      </div>
      <div class="labels">
        <div v-for="(label, index) in this.labels" :key="index">
        <label :for="label.index">{{ label }}</label>
        </div>
      </div>
    </span>
    <span class="sample" :class="theme">Theme : {{ theme }}</span>
  </div>
  </section>
</template>
<script>
export default {
  name: 'SetTheme',
  props: {
    defaultTheme: String,
  },
  data() {
    return {
      theme: null,
      values: [
        'happiness',
        'lagoon',
        'blossom',
        'moon',
        'goodday',
      ],
      labels: [
        'Happiness',
        'Lagoon',
        'Blossom',
        'Moon',
        'GoodDay'
      ],
    }
  },
  computed: {
    /* propsが最初に持っているテーマのindexを返す */
    defaultCheck: function() {
      for (let i=0; i<this.values.length; i++){
        if(this.defaultTheme === this.values[i]) {
          return i
        }
      } return 'Default'
    },
    /* 初期テーマを読む */
    newTheme: function() {
      if (this.defaultCheck === 'Default') {
        return this.defaultCheck
      } else return this.values[this.defaultCheck]
    }
  },
  methods: {
    /* 編集後テーマをemit */
    choseTheme() {
      this.$emit('cardsTheme', this.theme)
    },
    /* 既存テーマをセット */
    setEditingTheme() {
      return this.theme = this.newTheme
    },
    /* テーマを初期化 */
    clearTheme() {
      this.theme = null
    }
  }
}
</script>
<style scoped>
input:hover, label:hover {
  cursor: pointer;
}
.chosen-theme {
  font-size: 0.6rem;
  position: relative;
  display: block;
  text-align: center;
  margin: 1rem 0 2rem 0;
}
.inputs {
  width: 60%;
  display: flex;
  justify-content: space-evenly;
  overflow: hidden;
}
.labels {
  width: 60%;
  display: flex;
  justify-content: space-evenly;
  padding-right: 0.2rem;
  overflow: hidden;
}
.sample {
  width: 30%;
  display: block;
  padding: 0.5rem;
  font-weight: bold;
  border: 1px dotted rgb(70, 70, 70);
  position: absolute;
  top: 0;
  right: 0;
  height: 2rem;
}
</style>