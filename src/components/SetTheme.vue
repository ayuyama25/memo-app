<template>
  <section>
    <div class="choice-theme">
      <span class="sample" :class="newTheme">Set Theme? : {{ newTheme }}</span>
      <form class="inputs" @change="choseTheme">
        <label>
          <input type="radio" name="themes" :id="this.values[0]" :value="this.values[0]" v-model="newTheme">{{ labels[0] }}
        </label>
        <label>
          <input type="radio" name="themes" :id="this.values[1]" :value="this.values[1]" v-model="newTheme">{{ labels[1] }}
        </label>
        <label>
          <input type="radio" name="themes" :id="this.values[2]" :value="this.values[2]" v-model="newTheme">{{ labels[2] }}
        </label>
        <label>
          <input type="radio" name="themes" :id="this.values[3]" :value="this.values[3]" v-model="newTheme">{{ labels[3] }}
        </label>
        <label>
          <input type="radio" name="themes" :id="this.values[4]" :value="this.values[4]" v-model="newTheme">{{ labels[4] }}
        </label>
      </form>
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
        'happiness',
        'lagoon',
        'blossom',
        'gentlemoon',
        'goodday',
      ],
      labels: [
        'Happiness',
        'Lagoon',
        'Blossom',
        'GentleMoon',
        'GoodDay'
      ],
    }
  },
  computed: {
    /* propsが最初に持っているテーマのindexを返す */
    defaultCheck: function() {
      for (let i=0; i<this.values.length; i++){
        if(this.havingTheme === this.values[i]) {
          return i
        }
      } return 'Default'
    },
    /* 初期テーマを取得 */
    firstTheme: function() {
      if (this.defaultCheck === 'Default') {
        return this.defaultCheck
      } else return this.values[this.defaultCheck]
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
input:hover, label:hover {
  cursor: pointer;
}
.chosen-theme {
  font-size: 0.6rem;
  line-height: 1.1rem;
  position: relative;
  box-sizing: border-box;
  display: block;
  text-align: center;
  margin: 1rem 0;
  overflow: hidden;
}
.choice-theme form {
  display: inline-block;
  vertical-align: top;
  width: 60%;
  text-align: left;
}
.choice-theme label {
  white-space: nowrap;
  padding-right: 0.5rem;
}
.sample {
  display: inline-block;
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