<template>
  <div class="autoStr">
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="20rem" viewBox="0, 0, 100, 100" preserveAspectRatio="none">
    <path :d="pathStr" stroke="#fff" stroke-width="0.4" fill="none"></path>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'BackGroundString',
  data() {
    return {
      yValues: [],   // Y座標の配列
      pointsCount : 30,   //座標点の数
      maxY : 18,   //山の最大値
      widthSVG: 100,   //全体の幅
      heightSVG: 100,  //全体の高さ
      ease: 1.4,  //曲がり具合
    }
  },
  computed: {
    pathStr() {
      return this.valuesToPathStr(this.yValues)
    },
    points() {
      return this.yValues.map((y, x) => ({
        x: x / (this.pointsCount -1) * this.widthSVG,
        y: y * this.maxY + this.heightSVG / 2
      }))
    },
    /* 制御点の算出 */
    controlX() {
      return this.widthSVG / (this.pointsCount - 1) * this.ease
    }
  },
  methods: {
    nextY() {
      this.yValues = this.generateValues()
    },
    /* 0〜1と(-1)〜0の乱数で交互に埋めた値配列を生成 */
    generateValues() {
      return new Array(this.pointsCount + 1).fill(0).map((_, index) => Math.random() * ((index % 2) ? 1 : -1 ))
    },
    /* パス（ぺジェ曲線）を描画するための文字列を生成 */
    valuesToPathStr() {
      if (this.yValues.length < 2) {
        return 'M0,0'
      }
        return `M${this.points.shift().x},${this.points.shift().y} S` + this.points.map(p => `${p.x - this.controlX},${p.y} ${p.x},${p.y}`).join(' ')
    }
  },
  mounted() {
      this.nextY()
      window.setInterval(this.nextY, 1000)    
  }
}
</script>

<style scoped>
.autoStr {
  width: 100vw;
}
</style>