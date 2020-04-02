<template>
  <div class="container">
    <template v-for="x in board.length">
      <div
        v-for="y in board[x - 1].length"
        :key="`${y}-${x}`"
        class="cell"
        @click="onClick(x - 1, y - 1)"
      >
        <div
          v-if="isStone(x, y)"
          :class="['stone', isBlack(x, y) ? 'black' : 'white']"
        />
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'

interface Cell {
  x: number
  y: number
  color: number
}

@Component
export default class extends Vue {
  get isStone() {
    return (x: number, y: number): boolean => this.board[x - 1][y - 1] !== 0
  }

  get isBlack() {
    return (x: number, y: number): boolean => this.board[x - 1][y - 1] === 1
  }

  get putCells(): Cell[] {
    return this.board
      .flatMap((row, y) => row.map((color, x) => ({ x, y, color })))
      .filter((cell) => {
        return !!cell
      })
  }

  // board_content
  // 移動する方向
  directions = [
    [1, 1], // 右下
    [1, 0], // 右
    [1, -1], // 右上
    [0, 1], // 下
    [0, -1], // 上
    [-1, 1], // 左下
    [-1, 0], // 左
    [-1, -1] // 左上
  ]

  board = [
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 1, -1, 0, 0, 0],
    [0, 0, 0, -1, 1, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0]
  ]

  currentColor = 1

  onClick(x: number, y: number) {
    if (this.putCells.find((cell) => cell.x === x && cell.y === y)) {
      this.currentColor = 3 - this.currentColor
      this.board = JSON.parse(JSON.stringify(this.board)) // 再レンダリング処理
      this.board[y][x] = this.currentColor // イミュータブル 石の配置
      this.passTurn()
    }
  }

  passTurn() {
    // 手番の変更
    this.currentColor = this.currentColor * -1
  }
}
</script>

<style scoped>
/* autoprefixer grid: no-autoplace */
.container {
  width: 640px;
  height: 640px;
  margin: 20px auto;
  background: #050;
}
.cell {
  float: left;
  width: 12.5%;
  height: 12.5%;
  border: #000 solid;
}
.stone {
  width: 70%;
  height: 70%;
  margin: 15%;
  border-radius: 50%;
}
.black {
  background: #000;
}
.white {
  background: #fff;
}
</style>
© 2020 GitHub, Inc.
