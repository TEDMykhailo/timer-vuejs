<template>
  <div>
    <input
      @input="newTimer = $event.target.value"
      v-on:keyup.13="addTimer"
      type="text"
      placeholder="Create a new timer..."
    />
    <input type="submit" @click="addTimer" value="submit"/>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        newTimer: "",
      };
    },
    methods: {
      addTimer() {
        if (this.newTimer.length > 0) {
          this.$emit("on-new-timer", this.newTimer);
        }
        this.newTimer = "default timer ";
      }
    },
  };
</script>

