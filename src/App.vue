<template>
  <div
    class="container max-w-sm p-5 mx-auto text-2xl font-medium rounded shadow"
  >
    <h1>
      Next player: <span class="uppercase">{{ next_player }}</span>
    </h1>

    <ul class="my-5">
      <li
        class="flex flex-wrap"
        v-for="(click_value, index) in click_values"
        :key="index"
      >
        <button
          v-for="(j, idx) in click_value"
          :key="idx"
          @click="clickButton(index, idx)"
          :disabled="j.length || isSelect"
          :class="{ 'bg-red-200': isSelect, 'cursor-not-allowed': isSelect }"
          class="flex items-center justify-center w-10 h-10 px-5 py-4 uppercase border hover:bg-gray-100 active:bg-gray-200 disabled:bg-red-50"
        >
          {{ j }}
        </button>
      </li>
    </ul>
    <div>
      <h2>Result: {{ win }} player won</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      click_values: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      count: 0,
      win: ' ',
      next_player: 'x',
      isSelect: false
    }
  },
  methods: {
    clickButton(i, j) {
      if (this.count % 2 == 0) {
        this.click_values[i][j] = 'x'
        this.count++
        this.next_player = '0'
        this.winPlayer()
        this.isWin()
      } else {
        this.click_values[i][j] = '0'
        this.count++
        this.next_player = 'x'
        this.winPlayer()
        this.isWin()
      }
    },
    winPlayer() {
      // 1
      if (
        this.click_values[0][0] == this.click_values[0][1] &&
        this.click_values[0][0] == this.click_values[0][2] &&
        this.click_values[0][0] != ''
      ) {
        this.win = this.click_values[0][0]
      }
      // 2
      if (
        this.click_values[1][0] == this.click_values[1][1] &&
        this.click_values[1][0] == this.click_values[1][2] &&
        this.click_values[1][0] != ''
      ) {
        this.win = this.click_values[1][0]
      }
      // 3
      if (
        this.click_values[2][0] == this.click_values[2][1] &&
        this.click_values[2][0] == this.click_values[2][2] &&
        this.click_values[2][0] != ''
      ) {
        this.win = this.click_values[2][0]
      }
      // 4
      if (
        this.click_values[0][0] == this.click_values[1][1] &&
        this.click_values[0][0] == this.click_values[2][2] &&
        this.click_values[0][0] != ''
      ) {
        this.win = this.click_values[0][0]
      }
      // 5
      if (
        this.click_values[0][2] == this.click_values[1][1] &&
        this.click_values[0][2] == this.click_values[2][0] &&
        this.click_values[0][2] != ''
      ) {
        this.win = this.click_values[0][2]
      }
      // 6
      if (
        this.click_values[0][0] == this.click_values[1][0] &&
        this.click_values[0][0] == this.click_values[2][0] &&
        this.click_values[0][0] != ''
      ) {
        this.win = this.click_values[0][0]
      }
      // 7
      if (
        this.click_values[0][1] == this.click_values[1][1] &&
        this.click_values[0][1] == this.click_values[2][1] &&
        this.click_values[0][1] != ''
      ) {
        this.win = this.click_values[0][1]
      }
      // 8
      if (
        this.click_values[0][2] == this.click_values[1][2] &&
        this.click_values[0][2] == this.click_values[2][2] &&
        this.click_values[0][2] != ''
      ) {
        this.win = this.click_values[0][2]
      }
    },
    isWin() {
      if (this.win == 'x' || this.win == '0') {
        this.isSelect = true
      }
    }
  }
}
</script>
