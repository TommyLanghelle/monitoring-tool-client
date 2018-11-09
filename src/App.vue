<template>
    <div class="grid-container">
      <div class="grid-item" v-for="(frame, index) in frames" v-bind:key="index">
        <status-frame :ref="'status_frame_'+frame.id" :config="frame"></status-frame>
      </div>
    </div>
</template>

<script>
import statusFrame from "./components/status-frame.vue";
export default {
  components: { statusFrame },
  name: "app",
  data() {
    return {
      websocket: null,
      frames: []
    };
  },
  methods: {
    populateFrames(frames) {
      return new Promise(resolve => {
        if (this.frames.length > 0) {
          resolve();
          return;
        }
        for (let frame of frames) {
          this.frames.push(frame);
        }
        resolve();
      });
    }
  },
  created() {
    let _this = this;
    this.webSocket = new WebSocket("ws://localhost:8765");
    this.webSocket.onmessage = event => {
      var date = new Date();
      var data = JSON.parse(event.data);
      this.populateFrames(data).then(success => {
        for (var frameInfo of data) {
          if (!this.$refs["status_frame_" + frameInfo.id]) {
            continue;
          }

          var value = frameInfo.status ? 1 : 0;
          _this.$refs["status_frame_" + frameInfo.id][0].addPointToSeries(
            date.getTime(),
            value
          );
        }
      });
    };
    this.webSocket.onclose = e => {
      console.log("connectionclose", e);
    };
    this.webSocket.onerror = () => {
      console.log("connectionerror");
    };
    this.webSocket.onopen = () => {
      console.log("connectionOpened");
    };
  }
};
</script>

<style lang="postcss" scoped>
.grid-container {
  display: flex;
  flex-wrap: wrap;
  background-color: lightgrey;
  padding: 10px;
}
.grid-item {
  background-color: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.8);
  margin: 10px;
  padding: 20px;
  font-size: 30px;
  text-align: center;
}
</style>
