<template>
  <div class="py-10 bg-gray-800">
    <!-- GAME-UI-HEADER -->
    <header>
      <div class="container flex max-w-lg py-2 mx-auto">
        <!-- <x-mark-icon class="w-10 h-10 text-teal-600" /> -->
        <!-- <stop-circle-icon class="w-10 h-10 text-orange-500" /> -->
        <div class="flex items-center">
          <!-- <label
            for="game_size"
            class="block mb-2 mr-2 text-sm font-medium text-white whitespace-nowrap"
            >Game size</label
          > -->
          <select
            id="game_size"
            class="bg-gray-900 border text-white text-sm rounded-lg block w-full p-2.5 border-gray-800 outline-none placeholder-gray-400"
          >
            <option selected disabled>size</option>
            <option :value="3">3</option>
            <option :value="4">4</option>
            <option :value="5">5</option>
            <option :value="6">6</option>
          </select>
        </div>
      </div>
    </header>
    <!-- GAME-UI-HEADER -->

    <!-- GAME-UI-MAIN -->
    <main>
      <section>
        <div class="container max-w-lg mx-auto">
          <ul class="flex flex-wrap gap-4 max-w-[180px]">
            <li
              v-for="(box, index) in boxes"
              :key="index"
              class="flex w-12 h-12 bg-gray-900 rounded shadow-2xl"
            >
              <button
                class="flex items-center justify-center w-full h-full"
                @click="submit(index)"
              >
                <x-mark-icon
                  v-if="flag && isHiddin"
                  class="w-10 h-10 text-teal-600"
                />
                <stop-circle-icon
                  v-if="!flag"
                  class="w-10 h-10 text-teal-600"
                />
              </button>
            </li>
          </ul>
        </div>
      </section>
    </main>
    <!-- GAME-UI-MAIN -->
  </div>
</template>

<script>
import { XMarkIcon, StopCircleIcon } from '@heroicons/vue/24/outline'

export default {
  name: 'GameUi',
  components: {
    XMarkIcon,
    StopCircleIcon
  },
  data() {
    return {
      boxes: ['', '', '', '', '', '', '', '', ''],
      flag: true,
      isHiddin: false
    }
  },
  computed: {
    isActive() {
      return this.flag
    }
  },
  methods: {
    submit(index) {
      this.isHiddin = true
      this.flag = !this.flag
      this.boxes[index] = this.isActive
    }
  }
}
</script>
