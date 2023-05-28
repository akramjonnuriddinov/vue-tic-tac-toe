<template>
  <div class="font-medium container shadow rounded p-5 mx-auto max-w-sm">
    <h1 class="text-2xl">
      Next player: <span class="uppercase">{{ 'x' }}</span>
    </h1>

    <ul class="py-5 w-[200px] flex flex-wrap">
      <li class="" v-for="(click_value, index) in click_values" :key="index">
        <button
          v-for="(j, idx) in click_value"
          :key="idx"
          @click="clickButton(index, idx)"
          class="border w-10 flex items-center justify-center text-2xl h-10 py-4 px-5 uppercase hover:bg-gray-100 active:bg-gray-200"
        >
          {{ j }}
        </button>
      </li>
    </ul>
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
      count: 0
    }
  },
  methods: {
    clickButton(i, j) {
      if (this.count % 2 == 0) {
        this.click_values[i][j] = 'x'
        this.count++
      } else {
        this.click_values[i][j] = '0'
        this.count++
      }
    }
  },
  mounted() {
    console.log(this.click_values)
  }
}
</script>
