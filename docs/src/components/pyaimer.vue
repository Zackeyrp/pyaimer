<template>
  <div class="timer-container" @click="toggleTimer">
    <div class="timer-display">
      <span v-if="!started">GO</span>
      <span v-else>{{ time }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      started: false,
      time: 0,
      intervalId: null,
    };
  },
  methods: {
    toggleTimer() {
      if (this.started) {
        clearInterval(this.intervalId);
        this.started = false;
        this.time = 0;
      } else {
        this.started = true;
        this.intervalId = setInterval(() => {
          this.time++;
        }, 1000);
      }
    },
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  },
};
</script>

<style scoped>
.timer-container {
  background-color: #4fc3f7;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.timer-display {
  background-color: #cbeefc;
  padding: 1em 2em;
  border-radius: 8px;
  font-size: 5rem;
  color: #263238;
  font-weight: bold;
  text-align: center;
}
</style>
