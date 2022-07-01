<template>
  <section>
    <h2 class="gradation">Here's your notes:</h2>
    <form class="ordering">
      <input type="radio" id="byStars" value="stars" v-model="sortCardsBy">
      <label for="byStars">Rating</label>
      <input type="radio" id="byDate" value="date" v-model="sortCardsBy" checked>
      <label for="byDate">Date</label>
    </form>

    <div class="row cards" :class="[cssTheme(card.themeColor), index === hoveredIndex ? 'hover' : '']" v-for="(card, index) in cardsArray" :key ="index" @mouseover="hovering(index)" @mouseleave="mouseOff">
      <li class="cardsTitle" :class="[index === hoveredIndex ? 'hoverTitle' : '']">{{ card.title }}</li>
      <li class="cardsDescription">{{ card.description }}</li>
      <li class="cardsRating gradation">{{ ratingData(card.rating) }}</li>
      <li class="cardsDate">{{ setYear(card.timestamp) }}/{{ setMonth(card.timestamp) }}/{{ setDay(card.timestamp) }} {{ setHour(card.timestamp) }}:{{ setMinute(card.timestamp) }}</li>

      <edit-memo :editingCard="cardsArray[index]" @editedCabin="getEditedMemo" class="editButton"></edit-memo>  
      <delete-memo :deletingCardId="cardsArray[index].id" @deletingCard="getDeletedId" class="deleteButton"></delete-memo>

    </div>
  </section>
</template>
<script>
import EditMemo from './EditMemo.vue'
import DeleteMemo from './DeleteMemo.vue'
export default {
  name: 'MemoCards',
  props: {
    cards: Array,
    optionTheme: String
  },
  data() {
    return {
      editedMemo : {},
      deletingId : {},
      sortCardsBy : 'date',
      hoveredIndex : null,
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
    /* 投稿日時の表示 */
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
    /* デフォルトテーマの読み込み */
    cssTheme(value) {
      if (value === 'Default'){
        return this.optionTheme
      } else return value
      },
    /* ホバーされたカードのみにCSSを付与 */
    hovering(value) {
      this.hoveredIndex = value
    },
    mouseOff() {
      this.hoveredIndex = null
    },
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
  position: relative;
  border-radius: 10px;
  padding-bottom: 1rem;
  box-shadow:  -10px 10px 10px #e4e4e4, 10px -10px 10px #ffffff;
}
.cardsTitle {
  display: block;
  position: relative;
  background-color: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10%);
  border-top: 1px solid rgba(255, 255, 255, 0.5);
  border-right: 1px solid rgba(255, 255, 255, 0.5);
  color: azure;
  text-align: center;
  margin: 0.5rem 0.5rem 0 0.5rem;
  padding: 3rem;
  border-radius: 5px;
  font-size: 1.4rem;
  font-weight: 600;
  text-shadow: -1px 1px 4px rgba(0, 0, 0, 0.3);
  box-shadow: -0.5rem 0.5rem 2rem -2rem rgb(70, 70, 70);
}
.cardsDescription {
  position: relative;
  display: block;
  text-align: center;
  padding: 1rem 3rem;
  font-size: 1rem;
  margin-bottom: 0.5rem;
}
.cardsRating {
  position: relative;
  left: 3rem;
  display: inline-block;
  font-size: 1.1rem;
  padding: 0.6rem 0;
  -webkit-text-stroke: 0.5px #fff;
}
.cardsDate {
  position: absolute;
  top: 5%;
  right: 5%;
  display: inline-block;
  color: azure;
  font-size: 0.8rem; 
  text-shadow: -1px 1px 4px rgba(0, 0, 0, 0.3);
}
/* カードへのマウスホバー */
.hover {
  transition: all 0.3s ease-out;
  box-shadow:  -15px 15px 25px #bebebe, 15px -15px 25px #ffffff;
}
.hoverTitle {
  background-color: rgba(255, 255, 255, 0.35);
  transition: all 0.3s ease-out;
}
/* 編集ボタンの位置、イベント */
.editButton {
  position: absolute;
  bottom: 2%;
  right: 5%;
  transition: all 0.2s ease-out;
}
/* 削除ボタンの位置、イベント */
.deleteButton {
  position: absolute;
  top: 2rem;
  right: 1rem;
  transition: all 0.1s ease-out;
}
</style>