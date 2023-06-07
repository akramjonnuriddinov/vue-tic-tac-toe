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
        :class="{
          'max-w-[122px]': game_size == 3,
          'max-w-[162px]': game_size == 4
        }"
      >
        <li v-for="(box, index) in boxes[game_size]" :key="index">
          <button
            :disabled="box.length || disabledBtn"
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
        3: ['', '', '', '', '', '', '', '', ''],
        4: ['', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '']
      },
      count: 0,
      flag: true,
      combinations: {
        3: [
          [0, 1, 2],
          [3, 4, 5],
          [6, 7, 8],
          [0, 4, 8],
          [2, 4, 6],
          [0, 3, 6],
          [1, 4, 7],
          [2, 5, 8]
        ],
        4: [
          [0, 1, 2, 3],
          [4, 5, 6, 7],
          [8, 9, 10, 11],
          [12, 13, 14, 15],

          [0, 5, 10, 15],
          [3, 6, 9, 12],

          [0, 4, 8, 12],
          [1, 5, 9, 13],
          [2, 6, 10, 14],
          [3, 7, 11, 15]
        ]
      },
      steps: [],
      game_size: 3,
      flags: []
    }
  },
  watch: {
    game_size() {
      this.steps = []
      this.restart()
    },
    steps(value) {
      console.log(value)
    }
  },
  computed: {
    isActive() {
      return this.flag ? 'X' : 'O'
    },
    win() {
      let isWin = false
      let winnedPlayer = false
      for (let i = 0; i < this.combinations[this.game_size].length; i++) {
        const combination = this.combinations[this.game_size][i]
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
  methods: {
    click(index) {
      this.boxes[this.game_size][index] = this.isActive
      this.flag = !this.flag
      this.steps.push([...this.boxes[this.game_size]])
      this.flags.push(this.flag)
    },
    router(index) {
      this.flag = this.flags[index]
      this.boxes[this.game_size] = this.steps[index]
      this.steps = this.steps.filter(
        (step) => this.steps.indexOf(step) <= index
      )
    },
    restart() {
      if (this.game_size == 3) {
        this.boxes[this.game_size] = ['', '', '', '', '', '', '', '', '']
      } else {
        this.boxes[this.game_size] = [
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          '',
          ''
        ]
      }
      this.steps = []
      this.flag = true
    }
  }
}
</script>
