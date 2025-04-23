<template>
  <div class="timer-container" :style="{ backgroundColor: bgColor }" @click="handleClick">
    <div v-if="showLock" class="lock-mark">🔒</div>
    <div v-else class="time-box">{{ formattedTime }}</div>
    <audio ref="audioPlayer" src="/deathHinana.mp3" preload="auto"></audio>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 0,
      intervalId: null,
      started: false,
      bgColor: '#007BFF',
      clickTimestamps: [],
      showLock: false
    };
  },
  computed: {
    formattedTime() {
      return (this.time / 10).toFixed(1);
    }
  },
  methods: {
    handleClick() {
      const now = Date.now();
      this.clickTimestamps.push(now);
      this.clickTimestamps = this.clickTimestamps.filter(ts => now - ts <= 1000);

      // 1秒以内に3回クリック → 初期化して終了
      if (this.clickTimestamps.length >= 3) {
        this.resetToInitialState();
        this.clickTimestamps = [];
        return;
      }

      // 赤背景 & 30秒未満 → ロック表示・音声再生・リセット
      if (this.started && this.bgColor === 'red' && this.time < 300) {
        this.showLock = true;
        this.$refs.audioPlayer.play();
        this.resetTimer(false); // 再スタートしない
        return;
      }

      // 通常のスタート処理
      if (!this.started) {
        this.startTimer();
      }

      // 色の切り替え（30秒未満）
      if (this.started && this.time < 300) {
        if (this.bgColor === '#007BFF') {
          this.bgColor = 'yellow';
        } else if (this.bgColor === 'yellow') {
          this.bgColor = 'red';
        }
      }

      // 30秒経過で通常リセット
      if (this.time >= 300) {
        this.resetTimer(true); // 再スタートする
      }
    },
    startTimer() {
      this.started = true;
      this.showLock = false;
      this.intervalId = setInterval(() => {
        this.time++;
      }, 100);
    },
    resetTimer(shouldRestart) {
      clearInterval(this.intervalId);
      this.time = 0;
      this.started = false;
      if (shouldRestart) {
        this.bgColor = 'yellow';
        this.startTimer();
      }
    },
    resetToInitialState() {
      clearInterval(this.intervalId);
      this.time = 0;
      this.bgColor = '#007BFF';
      this.started = false;
      this.showLock = false;
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
  user-select: none;
  cursor: pointer;
  transition: background-color 0.2s ease;
  position: relative;
}

.time-box {
  background-color: white;
  padding: 40px 60px;
  border-radius: 20px;
  font-size: 6rem;
  font-weight: 900;
  color: black;
  font-family: 'Courier New', Courier, monospace;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

.lock-mark {
  font-size: 8rem;
  color: white;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
}
</style>
