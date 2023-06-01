<template>
  <div class="container max-w-sm p-5 mx-auto text-xl">
    <h1>
      Next player: <span class="uppercase">{{ active }}</span>
    </h1>
    <div class="flex items-start gap-10">
      <div class="my-5" style="width: 126px; display: flex; flex-wrap: wrap">
        <button
          v-for="(box, index) in boxes"
          :key="index"
          @click="clickButton(index)"
          :disabled="box.length"
          :class="{
            'bg-red-200': win,
            'cursor-not-allowed': win
          }"
          class="flex items-center justify-center w-10 h-10 px-5 py-4 uppercase border hover:bg-gray-100 active:bg-gray-200 disabled:bg-red-50"
        >
          {{ box }}
        </button>
      </div>
      <div>
        <button
          class="px-3 py-1 ml-auto text-lg text-white bg-gray-600 rounded"
          @click="restart"
        >
          Restart
        </button>
        <div class="flex flex-wrap gap-2 w-[160px] py-1 text-sm">
          <button
            v-for="(step, index) in steps"
            :key="index"
            @click="route(index)"
            class="font-normal"
          >
            shag#{{ index + 1 }}
          </button>
        </div>
      </div>
    </div>
    <div>
      <h2 class="mb-2">
        Result:
        <span class="text-green-500" v-if="win">{{ win }}</span>
        <span class="text-green-500" v-else>None</span> player won
      </h2>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      boxes: ['', '', '', '', '', '', '', '', ''],
      combinations: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ],
      flag: true,
      steps: [],
      show: true
    }
  },
  watch: {
    boxes: {
      deep: true,
      handler(value) {
        this.steps.push([...value])
      }
    }
  },
  computed: {
    active() {
      return this.flag ? 'X' : 'O'
    },
    win() {
      let isWin = false
      let winnedPlayer = ''
      for (let i = 0; i < this.combinations.length; i++) {
        const combination = this.combinations[i]
        if (
          this.boxes[combination[0]] === this.boxes[combination[1]] &&
          this.boxes[combination[0]] == this.boxes[combination[2]]
        ) {
          isWin = true
          winnedPlayer = this.boxes[combination[0]]
          break
        }
      }
      return isWin ? winnedPlayer : false
    }
  },
  methods: {
    updateCounter() {
      this.counter++
    },
    clickButton(index) {
      this.show = true
      this.boxes[index] = this.active
      this.flag = !this.flag
    },
    restart() {
      this.steps = []
      this.boxes = ['', '', '', '', '', '', '', '', '']
      this.show = false
    },
    route(index) {
      this.boxes = this.steps[index]
      this.steps = this.steps.filter((step) => this.steps.indexOf(step) < index)
    }
  }
}
</script>
