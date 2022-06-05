<template>
  <!-- https://codesandbox.io/s/github/apertureless/vue-chartjs/tree/main/sandboxes/bar -->
  <Bar
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
import { Bar } from "vue-chartjs";
import json from "../assets/data.json";
import Gradient from "javascript-color-gradient";

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

export default {
  name: "BarChart",
  components: { Bar },
  props: {
    chartId: {
      type: String,
      default: "bar-chart",
    },
    datasetIdKey: {
      type: String,
      default: "label",
    },
    width: {
      type: Number,
      default: 1400,
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
      type: Object,
      default: () => {},
    },
    teams: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      json: json,
      chartOptions: {
        responsive: true,
      },
      formattedData: null,
    };
  },
  methods: {
    rand(frm, to) {
      return ~~(Math.random() * (to - frm)) + frm;
    },
  },
  computed: {
    data() {
      const colors = {
        nba: "#7144e9",
        mlb: "#000000",
        nfl: "#e97144",
        soccer: "#e9c444",
      };

      let newData = [];

      if (Object.values(this.teams).includes("MLB")) {
        this.json.forEach((el, idx) => {
          if (el["Sports Team"].includes("MLB")) {
            newData.push(this.json[idx]);
          }
        });
      }
      if (Object.values(this.teams).includes("NBA")) {
        this.json.forEach((el, idx) => {
          if (el["Sports Team"].includes("NBA")) {
            newData.push(this.json[idx]);
          }
        });
      }
      if (Object.values(this.teams).includes("NFL")) {
        this.json.forEach((el, idx) => {
          if (el["Sports Team"].includes("NFL")) {
            newData.push(this.json[idx]);
          }
        });
      }
      if (Object.values(this.teams).includes("Soccer")) {
        this.json.forEach((el, idx) => {
          if (el["Sports Team"].includes("Soccer")) {
            newData.push(this.json[idx]);
          }
        });
      }

      this.json.forEach((el) => {
        if (el["Sports Team"].includes("NBA")) {
          el.color = colors.nba;
        } else if (el["Sports Team"].includes("MLB")) {
          el.color = colors.mlb;
        } else if (el["Sports Team"].includes("Soccer")) {
          el.color = colors.soccer;
        } else {
          el.color = colors.nfl;
        }
      });

      newData.sort((a, b) => {
        return b.Value - a.Value;
      });
      return newData;
    },
    colors() {
      return new Gradient()
        .setColorGradient("#3F2CAF", "e9446a")
        .setMidpoint(this.data.length)
        .getColors();
    },
    chartData() {
      return {
        labels: this.data.map((el) => {
          return el["Sports Team"];
        }),
        datasets: [
          {
            label: "Value in Billions",
            backgroundColor: this.data.map((el) => {
              return el.color;
            }),
            data: this.data.map((el) => {
              return el["Value"];
            }),
          },
        ],
      };
    },
  },
};
</script>
