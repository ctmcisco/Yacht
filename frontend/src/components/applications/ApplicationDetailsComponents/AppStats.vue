<template>
  <v-card raised>
    <v-card-title class="primary font-weight-bold">
      Stats
    </v-card-title>
    <v-card-text
      v-if="app.State.Status != 'running'"
      class="secondary text-center px-5 py-5"
    >
      Start the app to view stats
    </v-card-text>
    <div v-else>
      <v-card flat>
        <v-card-title>
          CPU Usage {{ stats.cpu_percent[stats.cpu_percent.length - 1] }}%
        </v-card-title>
        <v-card-subtitle>
          (0-100%) <br />
          Max: {{ Math.max.apply(Math, stats.cpu_percent) }}%
        </v-card-subtitle>
        <PercentLineChart :chartData="fillCPU(stats.cpu_percent, stats.time)" />
      </v-card>
      <v-card flat>
        <v-card-title>
          Memory Usage {{ stats.mem_percent[stats.mem_percent.length - 1] }}%,
          {{ formatBytes(stats.mem_current[stats.mem_current.length - 1]) }}/{{
            formatBytes(stats.mem_total[stats.mem_total.length - 1])
          }}
        </v-card-title>
        <v-card-subtitle>
          (0-100%) <br />
          Max: {{ Math.max.apply(Math, stats.mem_percent) }}%,
          {{ formatBytes(Math.max.apply(Math, stats.mem_current)) }}/{{
            formatBytes(stats.mem_total[stats.mem_total.length - 1])
          }}
        </v-card-subtitle>
        <PercentLineChart :chartData="fillMem(stats.mem_percent, stats.time)" />
      </v-card>
    </div>
  </v-card>
</template>

<script>
import PercentLineChart from "../../charts/PercentLineChart";
export default {
  components: {
    PercentLineChart,
  },
  props: ["app", "stats"],
  data() {
    return {};
  },
  methods: {
    formatBytes(bytes) {
      if (bytes === 0) return "0 Bytes";
      const decimals = 2;
      const k = 1024;
      const dm = decimals < 0 ? 0 : decimals;
      const sizes = ["Bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB"];

      const i = Math.floor(Math.log(bytes) / Math.log(k));

      return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + " " + sizes[i];
    },
    fillCPU(stat, time) {
      let datacollection = {
        datasets: [
          {
            label: "CPU Usage",
            backgroundColor: "#41b883",
            lineTension: 0,
            pointRadius: 0,
            data: time.map((t, i) => {
              return { x: t, y: stat[i] };
            }),
          },
        ],
      };
      return datacollection;
    },
    fillMem(stat, time) {
      let datacollection = {
        datasets: [
          {
            label: "Memory Usage",
            backgroundColor: "#41b883",
            lineTension: 0,
            pointRadius: 0,
            data: time.map((t, i) => {
              return { x: t, y: stat[i] };
            }),
          },
        ],
      };
      return datacollection;
    },
  },
};
</script>

<style></style>
