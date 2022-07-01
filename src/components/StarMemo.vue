<template>
  <section>
    <div>Set stars?</div>
    <div class="starRating">
      <span v-for="(item, index) in starList" :key="index" @change="changingRate(item.value)">
        <label :class="item.color"><input type="radio" name="stars" v-model="starsOfRate" :value="item.value">★</label>
      </span>
      <button class="clear-star-button" @click.prevent="clearStars">erase</button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'StarMemo',
  props: {
    havingStar : null,
  },
  data() {
    return {
      starsOfRate: null,
      starList: [
        {value: 1, name: '1star', color: ''},
        {value: 2, name: '2star', color: ''},
        {value: 3, name: '3star', color: ''},
        {value: 4, name: '4star', color: ''},
        {value: 5, name: '5star', color: ''},
      ] 
    }
  },
  computed: {
    /* 初期レートを取得 */
    firstStar: function() {
      if (1 <= this.havingStar <= 5) {
        return this.havingStar
      } else return null
    },
  },
  methods: {
    /* 選択変更時の動作 */
    changingRate(value) {
      this.colorStars(value)
      this.giveStars()
    },
    /* 初期レートを表示 */
    setRateStars() {
      this.starsOfRate = this.firstStar
      this.colorStars(this.starsOfRate)
      this.giveStars()
    },
    /* 選択したレートに応じて色をつける */
    colorStars(value) {
      for (let i=0; i<this.starList.length; i++) {
        this.starList[i].color = ''
      } 
      for (let i=0; i<value; i++) {
        this.starList[i].color = 'coloring-star'
      } return
    },
    /* レートと表示の初期化 */
    clearStars() {
      this.starsOfRate = null
      this.giveStars()
      for (let i=0; i<this.starList.length; i++) {
        this.starList[i].color = ''
      } return
    },
    /* 選択したレートをemit */
    giveStars() {
      this.$emit('starCabin', this.starsOfRate)
    },
  },
}
</script>

<style scoped>
section {
  padding: 0.5rem 0;
}
.starRating {
  display: block;
  flex-direction: row-reverse;
  overflow: hidden;
  position: relative;
  width: 80%;
  height: 100%;
  font-size: 2.4rem;
  margin: 0 auto;
}
.starRating input {
  display: none;
}
.starRating label {
  display: inline-block;
  box-sizing: border-box;
  width: 20%;
  position: relative;
}
.starRating label:hover {
  -webkit-text-stroke: 1px #fff;
}
/* 選択したスターに付与する */
.coloring-star{
  color: #c8ed7d;
}
/* eraseボタン */
.clear-star-button {
  position: relative;
  margin-top: 0.5rem;
  padding: 0 0.5rem;
  float: right;
}
</style>