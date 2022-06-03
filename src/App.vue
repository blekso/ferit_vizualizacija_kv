<template>
  <div class="my-24 grid gap-12">
    <!-- <div class="mx-24 mb-8 px-24 text-5xl font-bold">
      <Multiselect
        v-model="value"
        mode="multiple"
        placeholder="Select sport"
        :close-on-select="false"
        :options="{
          MLB: 'MLB',
          NBA: 'NBA',
          NFL: 'NFL',
          Soccer: 'Soccer',
        }"
      >
      </Multiselect>
    </div> -->
    <!-- <div>
      <div
        class="mx-12 h-12 flex bg-gray-500 justify-center items-center text-white cursor-pointer"
        @click="clearSelection"
      >
        <p>RESET</p>
      </div>
    </div> -->
    <div>
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">Select Sport</h1>
      <div class="grid grid-cols-4 gap-12 mx-24 p-12 h-64">
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('MLB') ? 'border-4 border-red-500' : ''"
          :style="styleMLB"
          @click="chooseTeam('MLB')"
        >
          <p>MLB</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('NBA') ? 'border-4 border-red-500' : ''"
          :style="styleNBA"
          @click="chooseTeam('NBA')"
        >
          <p>NBA</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="this.value.includes('NFL') ? 'border-4 border-red-500' : ''"
          :style="styleNFL"
          @click="chooseTeam('NFL')"
        >
          <p>NFL</p>
        </div>
        <div
          class="flex justify-center items-center text-white cursor-pointer"
          :class="
            this.value.includes('Soccer') ? 'border-4 border-red-500' : ''
          "
          :style="styleSoccer"
          @click="chooseTeam('Soccer')"
        >
          <p>Soccer</p>
        </div>
      </div>
    </div>
    <div v-if="value.length > 0">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">
        Most Valuable Teams Per Sport
      </h1>
      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <BarChart :teams="value" />
        </div>
      </div>
    </div>

    <!-- <div v-if="Object.values(value).includes('MLB')">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">MLB</h1>
      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChartMLB />
        </div>
      </div>
    </div>
    <div v-if="Object.values(value).includes('NBA')">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">NBA</h1>
      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChartNBA />
        </div>
      </div>
    </div>
    <div v-if="Object.values(value).includes('NFL')">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">NFL</h1>
      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChartNFL />
        </div>
      </div>
    </div>
    <div v-if="Object.values(value).includes('Soccer')">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">Soccer</h1>
      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChartSoccer />
        </div>
      </div>
    </div> -->
    <div v-if="value.length > 0">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">
        Average Five-Year Change In Value Per Sport
      </h1>

      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChartChange :teams="value" />
        </div>
      </div>
    </div>
    <div v-if="value.length > 0">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">Most Valuable Sport</h1>

      <div
        style="height: 1000px"
        class="flex justify-center items-center gap-12 mx-24 p-12 rounded-lg border-2 border-black"
      >
        <div class="max-w-2xl">
          <DoughnutChart :teams="value" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import Multiselect from "@vueform/multiselect";
import BarChart from "./components/BarChart.vue";
import DoughnutChart from "./components/DoughnutChart.vue";
import DoughnutChartChange from "./components/DoughnutChartChange.vue";

export default {
  name: "App",
  components: {
    BarChart,
    DoughnutChart,
    DoughnutChartChange,
    //Multiselect,
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

<style src="@vueform/multiselect/themes/default.css"></style>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
