<template>
  <div
    class="container max-w-sm p-5 mx-auto text-2xl font-medium rounded shadow"
  >
    <h1>
      Next player: <span class="uppercase">{{ next_player }}</span>
    </h1>

    <ul class="my-5" style="width: 126px; display: flex; flex-wrap: wrap">
      <button
          v-for="(j, index) in boxes"
          :key="index"
          @click="clickButton(index)"
          :disabled="j.length || isSelect"
          :class="{ 'bg-red-200': win, 'cursor-not-allowed': win }"
          class="flex items-center justify-center w-10 h-10 px-5 py-4 uppercase border hover:bg-gray-100 active:bg-gray-200 disabled:bg-red-50"
        >
          {{ j }}
        </button>
    </ul>
    <div>
      <h2 v-if="win">Result: {{ win }} player won</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      boxes: [
        '', '', '',
        '', '', '',
        '', '', ''
      ],
      combinations: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
      flag: true
    }
  },
  computed: {
    active () {
      return this.flag ? 'X' : 'O'
    },
    win () {
      let isWin = false
      let winnedPlayer = ''
      for (let i = 0; i < this.combinations.length; i++) {
        const combination = this.combinations[i]
        if (this.boxes[combination[0]] === this.boxes[combination[1]] && this.boxes[combination[0]] && this.boxes[combination[2]]) {
          isWin = true
          winnedPlayer = this.boxes[combination[0]]
          break
        }
      }

      return isWin ? winnedPlayer : false
    }
  },
  methods: {
    clickButton(index) {
      this.boxes[index] = this.active
      this.flag = !this.flag
    }
  }
}
</script>
