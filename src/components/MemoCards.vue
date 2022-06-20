<template>
  <section>
    <h2 class="gradation">Here's your notes:</h2>
    <form class="ordering">
      <input type="radio" id="byStars" value="stars" v-model="sortCardsBy" checked>
      <label for="byStars">Rating</label>
      <input type="radio" id="byDate" value="date" v-model="sortCardsBy">
      <label for="byDate">Date</label>
    </form>

    <div class="row cards" v-for="(card, index) in cardsArray" :key ="index">
      <div class="cardsTitle">{{ card.title }}</div>
      <div class="cardsDescription">{{ card.description }}</div>
      <div class="cardsRating gradation">{{ setRating(card.rating) }}</div>
      <div class="cardsDate">{{ setDate(card.timestamp) }}</div>

      <edit-memo :edittingCard="cardsArray[index]" @editedCabin="getEditedMemo" class="editButton"></edit-memo>  
      <!-- ↑編集：子コンポーネントに対象オブジェクトを渡し、編集後オブジェクトを受け取る -->
    </div>
  </section>
</template>
<script>
import EditMemo from './EditMemo.vue'
export default {
  name: 'MemoCards',
  props: {
    cards: Array
  },
  data() {
    return {
      editedMemo : {},
      date : {
        setDay : '',
        year: '',
        month: '',
        day: '',
        hours: '',
        minutes: ''
      },
      setStars : '',
      sortCardsBy : 'stars'
    }
  },
  computed: {
    /* propsの配列（表示用メモデータ）をコピーし、各基準の降順に並べ替え */
    cardsArray: function() {
      if (this.sortCardsBy === 'stars') {
        return Array.from(this.cards).sort((a,b) => 
        b.rating - a.rating )
      } else {
        return Array.from(this.cards).sort((a,b) => 
        b.timestamp - a.timestamp )
      }
    }
  },
  methods: {
    /* 日付表示変換 */
    setDate(value) {
      this.setDay = value
      this.year = this.setDay.getFullYear()
      this.month = this.setDay.getMonth()+1
      this.day = this.setDay.getDate()
      this.hours = this.setDay.getHours()
      this.minutes = this.setDay.getMinutes()
      this.setDay = null
      return (this.year +'/'+ this.month +'/'+ this.day +' '+ this.hours +':'+ this.minutes)
    },
    /* レート値を「★」表示に変換 */
    setRating(value) {
      this.setStars = value
      if (this.setStars == 5) {
          return '★★★★★'
        } else if (this.setStars == 4) {
          return '★★★★・'
        } else if (this.setStars == 3) {
          return '★★★・・'
        } else if (this.setStars == 2) {
        return '★★・・・'
        } else if (this.setStars == 1) {
        return '★・・・・'
      } else return '・・・・・'
    },
    /* 孫から編集後オブジェクトを受け取り、親にemit */
    getEditedMemo(value) {
      this.editedMemo = value
      this.$emit('editedCard', this.editedMemo)
      this.editedMemo = null
    }
  },
  components: {
    EditMemo
  }
}
</script>
<style scoped>
/* ソート基準選択ボタン */
.ordering {
  display: block;
  position: relative;
  text-align: right;
  padding: 1rem;
}
#byStars::before {
  content: "Sort";
  display: inline-block;
  position: relative;
  top: -1.3rem;
  left: 0;
  font-size: 0.6rem;
  text-align: center;
  white-space: nowrap;
}
/* カードのスタイル */
.cards {
  display: block;
  background-color: #fff;
  position: relative;
  border-radius: 5px;
  padding-bottom: 1rem;
}
.cardsTitle {
  display: block;
  position: relative;
  background-color: plum;
  color: azure;
  text-align: center;
  padding: 3rem;
  border-radius: 5px 5px 0 0;
  font-size: 1.4rem;
  font-weight: 600;
}
.cardsDescription {
  position: relative;
  display: block;
  text-align: center;
  padding: 1rem 3rem;
  font-size: 1rem;
  margin-bottom: 0.5rem;
  color: rgb(70, 70, 70)
}
.cardsRating {
  position: relative;
  left: 3rem;
  display: inline-block;
  font-size: 1rem;
  padding: 0.6rem 0;
  color: rgb(70, 70, 70)
}
.cardsDate {
  position: absolute;
  top: 5%;
  right: 5%;
  display: inline-block;
  font-size: 0.8rem; 
  color: rgb(70, 70, 70)
}
/* 編集ボタンの位置 */
.editButton {
  position: absolute;
  bottom: 2rem;
  top: 16%;
  right: 5%;
}
</style>