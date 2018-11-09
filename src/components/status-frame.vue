<template>
<div class="status-frame-wrapper">
  <highchart ref='highchart' :title="config.name"></highchart>
</div>
</template>
<script>
import highchart from "./highchart.vue";
export default {
  components: { highchart },
  props: {
    config: {
      name: "title"
    }
  },
  methods: {
    addPointToSeries(x, y) {
      this.$refs.highchart.addPointToSeries(x, y);
    }
  }
};
</script>
<style lang="postcss" scoped>
</style>
