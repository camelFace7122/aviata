<template>
  <button type="button" class="btn" :class="[type && `btn--${type}`]">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "CustomButton",
  props: {
    type: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
.btn {
  font-size: 18px;
  line-height: 24px;
  font-weight: 700;
  max-width: 200px;
  width: 100%;
  padding: 8px 12px;
  background-color: $c-green;
  border-radius: 4px;
  color: #ffffff;
  outline: none;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease-in;
  &:hover {
    background-color: darken($c-green, 5);
  }
  &--small {
    display: inline-block;
    width: auto;
    font-size: 12px;
    line-height: 16px;
    font-weight: 600;
    color: #5763b3;
    padding: 3px 8px;
    border-radius: 2px;
    background-color: #eaf0fa;
    &:hover {
      background-color: darken(#5763b3, 5);
      color: $c-white;
    }
  }
  &--link {
    display: inline-block;
    width: auto;
    font-size: 12px;
    line-height: 16px;
    font-weight: 400;
    padding: 0;
    background-color: transparent;
    color: #7284e4;
    border-bottom: 1px dashed rgba(#7284e4, 0.5);
    &:hover {
      background-color: transparent;
      color: darken(#5763b3, 5);
      border-color: transparent;
    }
  }
}
</style>
