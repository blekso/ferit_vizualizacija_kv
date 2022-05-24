<template>
  <div class="my-24 grid gap-12">
    <div class="mx-24 mb-8 px-24 text-5xl font-bold">
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
    </div>
    <div v-if="value.length > 0">
      <h1 class="mx-24 mb-8 px-24 text-5xl font-bold">
        Most Valuable Sports Teams
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
          <DoughnutChart :teams="value" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Multiselect from "@vueform/multiselect";
import BarChart from "./components/BarChart.vue";
import DoughnutChart from "./components/DoughnutChart.vue";

export default {
  name: "App",
  components: {
    BarChart,
    DoughnutChart,
    Multiselect,
  },
  data() {
    return {
      value: [],
    };
  },
};
</script>

<style src="@vueform/multiselect/themes/default.css"></style>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
