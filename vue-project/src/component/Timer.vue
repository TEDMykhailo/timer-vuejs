<template>
  <div class="i-timer">
    <div v-bind:title="name(description, position)" class="style-text-timer">{{ name(description, position) }}</div>
    <span
      v-bind:class="[state ? 'style-timer' : 'style-timer-pause']">{{ convert(localCurrentTime) }}</span>
    <button
      v-if="state"
      @click="$emit('on-stop')" class="on-stop medium">
      <span
        v-bind:class="[state ? 'mdi-pause' : 'mdi-play']"
        class="mdi mdi-pause style-stop"></span>
    </button>
    <button
      v-if="!state"
      @click="$emit('on-play')" class="on-play medium">
      <span
        v-bind:class="[state ? 'mdi-pause' : 'mdi-play']"
        class="mdi mdi-play style-play"></span>
    </button>
    <button @click="$emit('on-delete')" class="on-delete medium">
      <span class="mdi mdi-delete style-delete"></span>
    </button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        localCurrentTime: this.currentTime,
        timer: null,
      };
    },
    props: {
      state: Boolean,
      description: String,
      time: Number,
      position: Number,
      currentTime: Number,
      completed: Boolean
    },
    mounted() {
      this.startTimer()
    },
    destroyed() {
    },
    methods: {
      name(description, position) {
        return description === 'default timer ' ? description + ' ' + position : description;
      },
      convert(s) {
        let h, m, x, y, z;
        m = Math.floor(s / 60);
        s = s % 60;
        h = Math.floor(m / 60);
        m = m % 60;
        y = m > 9 ? m : '0' + m;
        h = h % 24;
        z = h > 9 ? h : '0' + h;
        // h = h === 0 ? '00' : h;
        x = s > 9 ? s : '0' + s;
        return z + ':' + y + ':' + x;
      },
      startTimer() {
        if (this.state) {
          this.timer = setInterval(() => {
            this.localCurrentTime++;
          }, 1000)
        }
      },
      stopTimer() {
        clearInterval(this.timer)
      },
    },
    watch: {
      currentTime(time) {
        if (time === 0) {
          this.stopTimer()
        }
      },
      state(state) {
        if (state) {
          this.startTimer();
        } else {
          this.stopTimer();
        }
      }
    },
  };
</script>
