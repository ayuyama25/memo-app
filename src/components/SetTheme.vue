<template>
  <section>
    <div class="choice-theme">
      <span class="sample" :class="newTheme">Set Theme? : {{ newTheme }}</span>
      <div class="inputs">
        <li v-for="(item, index) in values" :key="index" @change="choseTheme">
          <label><input type="radio" name="themes" :value="item.value" v-model="newTheme">{{ item.label }}</label>
        </li>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: 'SetTheme',
  props: {
    havingTheme: {}
  },
  data() {
    return {
      newTheme: null,
      values: [
        { value: 'happiness', label: 'Happiness' },
        { value: 'lagoon', label: 'Lagoon' },
        { value: 'blossom', label: 'Blossom' },
        { value: 'gentlemoon', label: 'GentleMoon' },
        { value: 'goodday', label: 'GoodDay' }
      ],
    }
  },
  computed: {
    /* propsが最初に持っているテーマのindexを返す */
    defaultCheck: function() {
      for (let i=0; i<this.values.length; i++){
        if(this.havingTheme === this.values[i].value) {
          return i
        }
      } return 'Default'
    },
    /* 初期テーマを取得 */
    firstTheme: function() {
      if (this.defaultCheck === 'Default') {
        return this.defaultCheck
      } else return this.values[this.defaultCheck].value
    }
  },
  methods: {
    /* 編集後テーマをemit */
    choseTheme() {
      this.$emit('cardsTheme', this.newTheme)
    },
    /* 編集時、既存テーマをセット */
    setEditingTheme() {
      this.newTheme = this.firstTheme
      this.choseTheme()
    },
    /* テーマを初期化 */
    clearTheme() {
      this.newTheme = null
    }
  }
}
</script>
<style scoped>
label:hover {
  cursor: pointer;
}
.choice-theme {
  font-size: 0.8rem;
  line-height: 2rem;
  position: relative;
  box-sizing: border-box;
  display: block;
  text-align: center;
  margin: 1rem 0;
  overflow: hidden;
}
.inputs {
  display: inline-block;
  vertical-align: top;
  width: 60%;
  text-align: left;
}
.inputs li {
  list-style: none;
  display: inline-block;
  padding-right: 0.5rem;
}
.inputs label {
  white-space: nowrap;
}
.sample {
  display: inline-block;
  box-sizing: border-box;
  vertical-align: top;
  position: relative;
  width: 30%;
  height: 100%;
  padding: 3%;
  margin-right: 3%;
  font-weight: bold;
  line-height: 1.3rem;
  border: 1px dotted rgb(70, 70, 70);
}
</style>