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
  <!-- <div>test</div> -->
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
    /* selectedTeamsTitle() {
      let str = "";
      this.teams.forEach((el) => {
        str += el + " ";
      });
      return str;
    }, */
    /* averagePerTeam() {
      let obj = {};
      if (Object.values(this.teams).includes("MLB")) {
        obj.mlb = this.data.mlb;
      }
      if (Object.values(this.teams).includes("NBA")) {
        obj.nba = this.data.nba;
      }
      if (Object.values(this.teams).includes("NFL")) {
        obj.nfl = this.data.nfl;
      }
      if (Object.values(this.teams).includes("Soccer")) {
        obj.soccer = this.data.soccer;
      }
      return obj;
    }, */
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
      };
      if (Object.values(this.teams).includes("MLB")) {
        let sum = 0;
        let count = 0;

        this.data.mlb.forEach((el) => {
          console.log(el["Five-Year Change In Value"]);
          sum += el["Five-Year Change In Value"];
          count++;
        });

        obj.sports.MLB = sum / count;
        obj.colors.MLB = colors.mlb;
      }
      if (Object.values(this.teams).includes("NBA")) {
        let sum = 0;
        let count = 0;

        this.data.nba.forEach((el) => {
          console.log(el["Five-Year Change In Value"]);
          sum += el["Five-Year Change In Value"];
          count++;
        });

        obj.sports.NBA = sum / count;
        obj.colors.NBA = colors.nba;
      }
      if (Object.values(this.teams).includes("NFL")) {
        let sum = 0;
        let count = 0;

        this.data.nfl.forEach((el) => {
          console.log(el["Five-Year Change In Value"]);
          sum += el["Five-Year Change In Value"];
          count++;
        });

        obj.sports.NFL = sum / count;
        obj.colors.NFL = colors.nfl;
      }
      if (Object.values(this.teams).includes("Soccer")) {
        let sum = 0;
        let count = 0;

        this.data.soccer.forEach((el) => {
          console.log(el["Five-Year Change In Value"]);
          sum += el["Five-Year Change In Value"];
          count++;
        });

        obj.sports.Soccer = sum / count;
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
    /* chartData() {
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
    }, */
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
