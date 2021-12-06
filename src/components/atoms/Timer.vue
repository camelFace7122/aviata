<template>
  <time class="timer" datetime="2004-07-24T18:18">
    <p class="timer__day text-small">{{ day.toLowerCase() }}</p>
    <p class="timer__time">{{ time }}</p>
  </time>
</template>

<script>
export default {
  props: {
    day: {
      type: String,
      default: "",
    },
    time: {
      type: String,
      default: "",
    },
    iso: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
.timer {
  display: inline-block;
  p {
    margin: 0;
  }
  &__time {
    margin-top: 5px;
    font-size: 24px;
    line-height: 32px;
    font-weight: 600;
  }
}
</style>
