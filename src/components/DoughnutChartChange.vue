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
import mlbJson from "../assets/mlb.json";
import nbaJson from "../assets/nba.json";
import nflJson from "../assets/nfl.json";
import soccerJson from "../assets/soccer.json";
import Gradient from "javascript-color-gradient";

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
  name: "DoughnutChart",
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
      default: 600,
    },
    height: {
      type: Number,
      default: 600,
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
    teams: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      data: {
        mlb: mlbJson,
        nba: nbaJson,
        nfl: nflJson,
        soccer: soccerJson,
      },
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
    averagePerSport() {
      const colors = {
        nba: "#7144e9",
        mlb: "e9446a",
        nfl: "#e97144",
        soccer: "#e9c444",
      };

      let obj = {
        sports: {},
        colors: {},
        values: {},
      };
      if (Object.values(this.teams).includes("MLB")) {
        let changeSum = 0;
        let valueSum = 0;
        let count = 0;

        this.data.mlb.forEach((el) => {
          changeSum += el["Five-Year Change In Value"];
          valueSum += el["Value"];
          count++;
        });

        obj.sports.MLB = changeSum / count;
        obj.values.MLB = valueSum;
        obj.colors.MLB = colors.mlb;
      }
      if (Object.values(this.teams).includes("NBA")) {
        let changeSum = 0;
        let valueSum = 0;
        let count = 0;

        this.data.nba.forEach((el) => {
          changeSum += el["Five-Year Change In Value"];
          valueSum += el["Value"];
          count++;
        });

        obj.sports.NBA = changeSum / count;
        obj.values.NBA = valueSum;
        obj.colors.NBA = colors.nba;
      }
      if (Object.values(this.teams).includes("NFL")) {
        let changeSum = 0;
        let valueSum = 0;
        let count = 0;

        this.data.nfl.forEach((el) => {
          changeSum += el["Five-Year Change In Value"];
          valueSum += el["Value"];
          count++;
        });

        obj.sports.NFL = changeSum / count;
        obj.values.NFL = valueSum;
        obj.colors.NFL = colors.nfl;
      }
      if (Object.values(this.teams).includes("Soccer")) {
        let changeSum = 0;
        let valueSum = 0;
        let count = 0;

        this.data.soccer.forEach((el) => {
          changeSum += el["Five-Year Change In Value"];
          valueSum += el["Value"];
          count++;
        });

        obj.sports.Soccer = changeSum / count;
        obj.values.Soccer = valueSum;
        obj.colors.Soccer = colors.soccer;
      }
      return obj;
    },
    colors() {
      return new Gradient()
        .setColorGradient("#3F2CAF", "e9446a")
        .setMidpoint(Object.keys(this.averagePerSport).length)
        .getColors();
    },
    chartData() {
      return {
        labels: Object.keys(this.averagePerSport.sports),
        datasets: [
          {
            label: "Five-Year Change In Value",
            backgroundColor: Object.values(this.averagePerSport.colors),
            data: Object.values(this.averagePerSport.sports),
          },
        ],
      };
    },
  },
};
</script>
