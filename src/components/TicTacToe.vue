<template>
  <div class="container max-w-md px-5 py-5 mx-auto text-xl">
    <div class="flex gap-10">
      <label for="x3">
        3x
        <input
          :value="3"
          v-model="game_size"
          id="x3"
          type="radio"
          name="game"
        />
      </label>
      <label for="x4">
        4x
        <input
          :value="4"
          v-model="game_size"
          id="x4"
          type="radio"
          name="game"
        />
      </label>
      <label for="x5">
        5x
        <input
          :value="5"
          v-model="game_size"
          id="x5"
          type="radio"
          name="game"
        />
      </label>
      <div>Game size: {{ game_size }}</div>
    </div>

    <p>Next player: {{ isActive }}</p>

    <div class="flex items-start justify-between">
      <ul class="flex flex-wrap border" :class="gameSizeClass">
        <li v-for="(box, index) in boxes" :key="index">
          <button
            :disabled="box.length || win"
            @click="click(index)"
            class="flex items-center justify-center w-10 h-10 text-xl border disabled:bg-gray-100"
          >
            {{ box }}
          </button>
        </li>
      </ul>
      <ul class="flex flex-wrap gap-2 text-base max-w-[100px]">
        <li v-for="(_, index) in steps" :key="index">
          <button @click="router(index)" class="underline">
            #{{ index + 1 }}
          </button>
        </li>
      </ul>
    </div>

    <div class="flex gap-5 pt-5">
      <div>
        Win:
        <span v-if="!win">None</span>
        <span v-else>{{ win }}</span>
      </div>
      <span>|</span>
      <button @click="restart">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
  data() {
    return {
      game_size: 3,
      boxes: this.createBoxes(3),
      count: 0,
      combinations: [],
      steps: []
    }
  },
  computed: {
    isActive() {
      return this.steps.length % 2 === 0 ? 'X' : 'O'
    },
    gameSizeClass() {
      const size = this.game_size * 40 + 2 || 'max-w-[122px]'
      return 'max-w-' + '[' + size + 'px]'
    },
    win() {
      let isWin = false
      let winnedPlayer = false
      for (let i = 0; i < this.combinations.length; i++) {
        const combination = this.combinations[i]
        let checkWin = true
        for (let j = 0; j < combination.length; j++) {
          if (this.boxes[combination[0]] !== this.boxes[combination[j]]) {
            checkWin = false
          }
        }
        if (checkWin && this.boxes[combination[0]] !== '') {
          isWin = true
          winnedPlayer = this.boxes[combination[0]]
          break
        }
      }
      return isWin ? winnedPlayer : false
    },
    disabledBtn() {
      if (this.win == '' || this.win == false) {
        return false
      }
      return true
    }
  },
  watch: {
    async game_size() {
      this.combinations = await this.createCombination(this.game_size)
      this.steps = []
      this.restart()
    }
  },
  async mounted() {
    this.combinations = await this.createCombination(this.game_size)
  },
  methods: {
    click(index) {
      this.boxes[index] = this.isActive
      this.steps.push([...this.boxes])
    },
    router(index) {
      this.boxes = [...this.steps[index]]
      this.steps = this.steps.filter(
        (step) => this.steps.indexOf(step) <= index
      )
    },
    async restart() {
      this.combinations = await this.createCombination(this.game_size)
      this.boxes = this.createBoxes(this.game_size)
      this.steps = []
    },
    createBoxes(size) {
      const boxes = []
      for (let i = 0; i < size * size; i++) {
        boxes[i] = ''
      }

      return boxes
    },
    createCombination(size) {
      const combinations = []
      let x = Number(0)
      for (let i = 0; i < size; i++) {
        const combination = []
        for (let j = 0; j < size; j++) {
          combination.push(j + x)
        }
        combinations.push(combination)
        x += size
      }

      for (let i = 0; i < size; i++) {
        const combination = []
        let y = 0
        for (let j = 0; j < size; j++) {
          combination.push(i + y)
          y += size
        }
        combinations.push(combination)
      }
      const combination1 = []
      for (let i = 0; i < size; i++) {
        for (let j = 0; j < size; j++) {
          if (i === j) {
            combination1.push((size + 1) * i)
          }
        }
      }
      combinations.push(combination1)

      const combination2 = []
      for (let i = 0; i < size; i++) {
        for (let j = 0; j < size; j++) {
          if (i + j === size - 1) {
            combination2.push((size - j) * (size - 1))
          }
        }
      }
      combinations.push(combination2)
      return [...combinations]
    }
  }
}
</script>
