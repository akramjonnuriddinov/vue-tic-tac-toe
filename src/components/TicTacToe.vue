<template>
  <div class="container max-w-md px-5 py-5 mx-auto text-xl">
    <div class="flex gap-10">
      <label for="x3">
        3x
        <input value="3" v-model="game_size" id="x3" type="radio" name="game" />
      </label>
      <label for="x4">
        4x
        <input value="4" v-model="game_size" id="x4" type="radio" name="game" />
      </label>
      <div>Game size: {{ game_size }}</div>
    </div>

    <p>Next player: {{ isActive }}</p>

    <div class="flex items-start justify-between">
      <ul
        class="flex flex-wrap border"
        :class="gameSizeClass"
      >
        <li v-for="(box, index) in boxes[game_size]" :key="index">
          <button
            :disabled="win"
            @click="click(index)"
            :class="{ 'bg-red-50': win }"
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
      boxes: {
        3: this.createBoxes(3),
        4: this.createBoxes(4)
      },
      count: 0,
      combinations: [],
      steps: [],
      game_size: 3,
    }
  },
  computed: {
    isActive () {
     return this.steps.length % 2 === 0 ? 'X' : 'O'
    },
    gameSizeClass () {
      const classes = {
        3: 'max-w-[122px]',
        4: 'max-w-[162px]'
      }
      return classes[this.game_size]
    },
    win() {
      let isWin = false
      let winnedPlayer = false
      for (let i = 0; i < this.combinations.length; i++) {
        const combination = this.combinations[i]
        let checkWin = true
        for (let j = 0; j < combination.length; j++) {
          if (
            this.boxes[this.game_size][combination[0]] !==
            this.boxes[this.game_size][combination[j]]
          ) {
            checkWin = false
          }
        }
        if (checkWin && this.boxes[this.game_size][combination[0]] !== '') {
          isWin = true
          winnedPlayer = this.boxes[this.game_size][combination[0]]
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
  async mounted () {
   this.combinations = await this.createCombination(this.game_size)
  },
  methods: {
    click(index) {
      this.boxes[this.game_size][index] = this.isActive
      this.steps.push([...this.boxes[this.game_size]])
    },
    router(index) {
      this.boxes[this.game_size] = [...this.steps[index]]
      this.steps = this.steps.filter(
        (step) => this.steps.indexOf(step) <= index
      )
    },
    async restart() {
      this.combinations = await this.createCombination(this.game_size)
      console.log(this.combinations)
      this.boxes[this.game_size] = this.createBoxes(this.game_size)
      this.steps = []
    },
    createBoxes (size) {
      const boxes = []
      for (let i = 0; i < size * size; i++) {
        boxes[i] = ''
      }

      return boxes
    },
    createCombination (size) {
      const combinations = []
      let x = 0
      for (let i = 0; i < size; i++) {
        const combination = []
        for (let j = 0; j< size; j++) {
          combination.push(j + x)
        }
        combinations.push(combination)
        x += size
      }

      for (let i = 0; i < size; i++) {
        const combination = []
        let y= 0
        for (let j = 0; j< size; j++) {
          combination.push(i + y)
          y += size
        }
        combinations.push(combination)
      }
      const combination1 = []
      for (let i = 0; i < size; i++) {
        for (let j = 0; j< size; j++) {
          if (i === j) {
            combination1.push((size + 1) * i)
          }
        }
      }
      combinations.push(combination1)

      const combination2 = []
      for (let i = 0; i < size; i++) {
        for (let j = 0; j< size; j++) {
          if ((i + j) === size -1 ) {
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
