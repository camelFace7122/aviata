<template>
  <div class="duration-indicator">
    <div class="duration-indicator__head">
      <span class="duration-indicator__dest">{{ depCode }}</span>
      <span class="text-small">{{ duration }}</span>
      <span class="duration-indicator__dest">{{ arrCode }}</span>
    </div>
    <div class="duration-indicator-line">
      <div class="duration-indicator-line__line"></div>
      <div class="duration-indicator-line__line-points">
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <p class="duration-indicator__helper-text text-small">
      <slot></slot>
    </p>
  </div>
</template>

<script>
export default {
  props: {
    duration: {
      type: String,
      default: "",
    },
    depCode: {
      type: String,
      default: "",
    },
    arrCode: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
.duration-indicator {
  padding: 0 20px;
  &__head {
    display: flex;
    justify-content: space-between;
  }
  &__dest {
    font-size: 10px;
    line-height: 12px;
    color: $c-gray;
  }
  &-line {
    position: relative;
    margin: 23px 0;
    @media (min-width: 580px) {
      margin: 4px 0;
    }
    &__line {
      height: 1px;
      background-color: $c-gray;
    }
    &__line-points {
      position: absolute;
      top: calc(50% - 3px);
      display: flex;
      width: 100%;
      justify-content: space-between;
      div {
        border: 1px solid $c-gray;
        width: 5px;
        height: 5px;
        background-color: $c-white;
        border-radius: 50%;
      }
    }
  }
  &__helper-text {
    text-align: center;
    color: $c-orange;
  }
}
</style>
