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
        { value: 'cotton', label: 'Cotton'},
        { value: 'blossom', label: 'Blossom' },
        { value: 'lagoon', label: 'Lagoon' },
        { value: 'goodday', label: 'GoodDay' },
        { value: 'midnight', label: 'Midnight' },
        { value: 'happiness', label: 'Happiness' },
        { value: 'Default', label: 'Default'} //末尾にDefaultを設置
      ],
      valuesLength: 7, // valuesのオブジェクト数が増減したら書き換える
      cashDefault: [],
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
    },
    /* 'Default'を選択肢から外す、戻す */
    removeDefault() {
      if(this.values.length === this.valuesLength-1) {
        return
      } else {
        this.cashDefault = this.values.pop()
      }
    },
    addDefault() {
      if(this.values.length === this.valuesLength) {
        return
      } else {
        this.values.push(this.cashDefault)
        this.cashDefault = []
      }
    },
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
  border: 1px dotted #464646;
  border-radius: 5px;
}
</style>
<style>
/* カードの色テーマ（共通） */
.happiness{
  background: #f9a239;
  background: -webkit-linear-gradient(to left, #40E0D0, #f9a239,#f53e9a); 
  background: linear-gradient(to left, #40E0D0, #f9a239,#f53e9a);
}
.lagoon {
  background: #71b0ef;
  background: -webkit-linear-gradient(to left, #74eba4, #71b0ef);
  background: linear-gradient(to left, #74eba4, #71b0ef);
}
.midnight {
  background: #593ae2;
  background: -webkit-linear-gradient(to left, #a8c0ff, #593ae2);
  background: linear-gradient(to left, #a8c0ff, #593ae2);
}
.blossom {
  background: #f348a3;
  background: -webkit-linear-gradient(to right, #f348a3, #FBD3E9);
  background: linear-gradient(to right, #f348a3, #FBD3E9);
}
.goodday {
  background: #F9D423;
  background: -webkit-linear-gradient(to left, #F9D423, #FF4E50);
  background: linear-gradient(to left, #F9D423, #FF4E50);
}
.cotton {
  background: #c7def0;
  background: -webkit-linear-gradient(to left, #eef2f3, #c7def0);
  background: linear-gradient(to left, #eef2f3, #c7def0);
}
</style>