<template>
  <Doughnut
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script>
import { Doughnut } from "vue-chartjs";
import json from "../assets/nba.json";

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  CategoryScale,
} from "chart.js";

ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

export default {
  name: "DoughnutChartNBA",
  components: {
    Doughnut,
  },
  props: {
    chartId: {
      type: String,
      default: "doughnut-chart",
    },
    datasetIdKey: {
      type: String,
      default: "label",
    },
    width: {
      type: Number,
      default: 800,
    },
    height: {
      type: Number,
      default: 800,
    },
    cssClasses: {
      default: "",
      type: String,
    },
    styles: {
      type: Object,
      default: () => {},
    },
    plugins: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      data: json,
      chartOptions: {
        legend: {
          display: false,
        },
        tooltips: {
          enabled: false,
        },
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  methods: {
    rand(frm, to) {
      return ~~(Math.random() * (to - frm)) + frm;
    },
  },
  computed: {
    colors() {
      var colors = [];
      while (colors.length < 100) {
        colors.push(
          `rgb(${this.rand(0, 255)}, ${this.rand(0, 255)}, ${this.rand(
            0,
            255
          )})`
        );
      }

      return colors.slice(50, 100);
    },
    chartData() {
      return {
        labels: this.data.map((el) => {
          return el["Sports Team"];
        }),
        datasets: [
          {
            label: "Five-Year Change In Value",
            backgroundColor: this.colors,
            data: this.data.map((el) => {
              return el["Five-Year Change In Value"];
            }),
          },
        ],
      };
    },
  },
};
</script>
