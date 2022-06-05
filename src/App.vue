<template>
  <div class="grid gap-4">
    <div class="bg-gray-200 flex justify-between items-center">
      <h1 class="p-4 text-2xl font-bold">Most Valuable Sports Teams</h1>
      <h1 class="p-4 text-2xl font-bold">Mihael Ištvan DRC 1.g</h1>
    </div>
    <div class="my-4">
      <h1 class="mx-12 mb-8 px-4 text-4xl font-bold">Select Sport</h1>
      <div class="grid grid-cols-4 gap-4 mx-12 p-4 h-32">
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('MLB') ? 'border-4 border-red-500' : ''"
          :style="styleMLB"
          @click="chooseTeam('MLB')"
        >
          <p class="text-3xl font-bold uppercase">MLB</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('NBA') ? 'border-4 border-red-500' : ''"
          :style="styleNBA"
          @click="chooseTeam('NBA')"
        >
          <p class="text-3xl font-bold uppercase">NBA</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('NFL') ? 'border-4 border-red-500' : ''"
          :style="styleNFL"
          @click="chooseTeam('NFL')"
        >
          <p class="text-3xl font-bold uppercase">NFL</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="
            this.value.includes('Soccer') ? 'border-4 border-red-500' : ''
          "
          :style="styleSoccer"
          @click="chooseTeam('Soccer')"
        >
          <p class="text-3xl font-bold uppercase">Soccer</p>
        </div>
      </div>
      <div class="grid lg:grid-cols-2 mx-4 gap-4">
        <div
          v-if="value.length > 0"
          class="border-2 border-black rounded-lg col-span-2"
        >
          <h1 class="mx-12 my-8 text-5xl font-bold">
            Most Valuable Teams Per Sport
          </h1>
          <div
            style="height: 900px"
            class="flex justify-center items-center mx-12"
          >
            <div>
              <BarChart :teams="value" />
            </div>
          </div>
        </div>
        <div v-if="value.length > 0" class="border-2 border-black rounded-lg">
          <h1 class="mx-12 mt-8 text-5xl font-bold" style="height: 100px">
            Average Five-Year Change In Value Per Sport
          </h1>

          <div
            style="height: 700px"
            class="flex justify-center items-center gap-12 mx-12 p-12"
          >
            <div>
              <DoughnutChartChange :teams="value" />
            </div>
          </div>
        </div>
        <div v-if="value.length > 0" class="border-2 border-black rounded-lg">
          <h1 class="mx-12 mt-8 text-5xl font-bold" style="height: 100px">
            Most Valuable Sport
          </h1>

          <div
            style="height: 700px"
            class="flex justify-center items-center gap-12 mx-12 p-12"
          >
            <div>
              <DoughnutChart :teams="value" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BarChart from "./components/BarChart.vue";
import DoughnutChart from "./components/DoughnutChart.vue";
import DoughnutChartChange from "./components/DoughnutChartChange.vue";

export default {
  name: "App",
  components: {
    BarChart,
    DoughnutChart,
    DoughnutChartChange,
  },
  data() {
    return {
      value: [],
      styleNBA: {
        backgroundColor: "#7144fe",
      },
      styleNFL: {
        backgroundColor: "#e97144",
      },
      styleMLB: {
        backgroundColor: "#000000",
      },
      styleSoccer: {
        backgroundColor: "#e9c444",
      },
    };
  },
  methods: {
    chooseTeam(team) {
      if (!this.value.includes(team)) {
        this.value = [...this.value, team];
      } else {
        let index = this.value.indexOf(team);
        this.value.splice(index, 1);
      }
    },
    clearSelection() {
      this.value = [];
    },
  },
};
</script>
