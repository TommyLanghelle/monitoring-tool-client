<template>
<div>
  <div id="container" ref="chart"></div>
</div>
</template>
<script>
import HighCharts from "highcharts";
export default {
  props: {
    title: String
  },
  data() {
    return {
      chart: null,
      baseSettings: {
        title: {
          text: "Missing title"
        },
        chart: {
          zoomType: "x"
        },
        title: {
          text: "TITLE"
        },
        xAxis: {
          type: "datetime"
        },
        yAxis: {
          title: {
            text: "Uptime"
          }
        },
        legend: {
          enabled: false
        },
        series: [
          {
            id: "uptime",
            name: "Uptime",
            data: []
          }
        ]
      }
    };
  },
  methods: {
    init(series) {
      this.baseSettings.title.text = this.title;
      this.chart = HighCharts.chart(this.$refs.chart, this.baseSettings);
    },
    addPointToSeries(x, y) {
      let series = this.chart.get("uptime");
      console.log(x, y);
      series.addPoint([x, y], true, true, true); // coords, redraw, shift, animation
    }
  },
  mounted() {
    this.init([]);
  }
};
</script>
<style lang="postcss" scoped>
#container {
  width: 450px;
  height: 200px;
}
</style>
