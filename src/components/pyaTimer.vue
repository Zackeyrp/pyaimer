<template>
  <div class="timer-container" @click="startTimer">
    <div class="time">{{ formattedTime }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 0,
      intervalId: null,
      started: false
    };
  },
  computed: {
    formattedTime() {
      const minutes = String(Math.floor(this.time / 60)).padStart(2, '0');
      const seconds = String(this.time % 60).padStart(2, '0');
      return `${minutes}:${seconds}`;
    }
  },
  methods: {
    startTimer() {
      if (this.started) return; // すでに始まっているなら何もしない
      this.started = true;
      this.intervalId = setInterval(() => {
        this.time++;
      }, 1000);
    }
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  }
};
</script>

<style scoped>
.timer-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #007BFF;
  color: white;
  font-size: 5rem;
  font-family: 'Courier New', Courier, monospace;
  user-select: none;
  cursor: pointer;
  text-align: center;
}
</style>
