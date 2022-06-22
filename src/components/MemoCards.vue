<template>
  <section>
    <h2 class="gradation">Here's your notes:</h2>
    <form class="ordering">
      <input type="radio" id="byStars" value="stars" v-model="sortCardsBy">
      <label for="byStars">Rating</label>
      <input type="radio" id="byDate" value="date" v-model="sortCardsBy" checked>
      <label for="byDate">Date</label>
    </form>

    <div class="row cards" v-for="(card, index) in cardsArray" :key ="index">
      <li class="cardsTitle">{{ card.title }}</li>
      <li class="cardsDescription">{{ card.description }}</li>
      <li class="cardsRating gradation">{{ ratingData(card.rating) }}</li>
      <li class="cardsDate">{{ setYear(card.timestamp) }}/{{ setMonth(card.timestamp) }}/{{ setDay(card.timestamp) }} {{ setHour(card.timestamp) }}:{{ setMinute(card.timestamp) }}</li>

      <edit-memo :editingCard="cardsArray[index]" @editedCabin="getEditedMemo" class="editButton"></edit-memo>  
      <delete-memo :deletingCardId="cardsArray[index].id" @deletingCard="getDeletedId"></delete-memo>

    </div>
  </section>
</template>
<script>
import EditMemo from './EditMemo.vue'
import DeleteMemo from './DeleteMemo.vue'
export default {
  name: 'MemoCards',
  props: {
    cards: Array
  },
  data() {
    return {
      editedMemo : {},
      deletingId : {},
      sortCardsBy : 'date'
    }
  },
  computed: {
    /* propsの配列（表示用メモデータ）をコピーし、各基準の降順に並べ替え */
    cardsArray: function() {
      if (this.sortCardsBy === 'date') {
        return Array.from(this.cards).sort((a,b) => 
        b.timestamp - a.timestamp )
      } else {
        return Array.from(this.cards).sort((a,b) => 
        b.rating - a.rating )
      }
    },
    setYear: () => (timedata) => timedata.getFullYear(),
    setMonth: () => (timedata) => timedata.getMonth()+1,
    setDay: () => (timedata) => timedata.getDate(),
    setHour: () => (timedata) => timedata.getHours(),
    setMinute: () => (timedata) => timedata.getMinutes().toString().padStart(2,'0'),
    ratingData: function() {
      return function(rateData) {
      if (rateData == 5) {
        return '★★★★★'
      } else if (rateData == 4) {
        return '★★★★・'
      } else if (rateData == 3) {
        return '★★★・・'
      } else if (rateData == 2) {
      return '★★・・・'
      } else if (rateData == 1) {
      return '★・・・・'
      } else return '・・・・・'
      }
    },
  },
  methods: {
    /* 孫から編集,削除オブジェクトを受け取り、親にemit */
    getEditedMemo(value) {
      this.editedMemo = value
      this.$emit('editedCard', this.editedMemo)
      this.editedMemo = null
    },
    getDeletedId(value) {
      this.deletingId = value
      this.$emit('deletedId', this.deletingId)
      this.deletingId = null
    },
  },
  components: {
    EditMemo,
    DeleteMemo
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
  display: inline-block;
  width: 100%;
  height: 100%;
  margin: 1rem auto;
  background-color: #fff;
  position: relative;
  border-radius: 5px;
  padding-bottom: 1rem;
  box-shadow: -0.5rem 0.5rem 2rem -2rem rgb(70, 70, 70);
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
  bottom: 2%;
  right: 5%;
}
</style>