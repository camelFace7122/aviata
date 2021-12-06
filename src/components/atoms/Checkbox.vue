<template>
  <label class="label-check">
    <input
      v-model="model"
      :checked="checked"
      type="checkbox"
      class="option-input checkbox"
      :name="name"
      :value="value"
    />
    <slot />
  </label>
</template>

<script>
export default {
  name: "Checkbox",
  model: {
    prop: "checked",
    event: "change",
  },
  props: {
    name: {
      type: String,
      default: "checkbox",
    },
    checked: {
      type: [Boolean, Array],
      default: false,
    },
    value: {
      type: [String, Number],
      default: "",
    },
  },
  computed: {
    model: {
      get() {
        return this.checked;
      },
      set(checked) {
        this.$emit("change", checked);
      },
    },
  },
};
</script>

<style lang="scss" scoped>
.label-check {
  display: flex;
  align-items: center;
  font-size: 12px;
  line-height: 16px;
}
.option-input {
  appearance: none;
  position: relative;
  height: 12px;
  width: 12px;
  transition: all 0.15s ease-out 0s;
  border: 0.5px solid rgba(55, 56, 58, 0.4);
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  margin-right: 12px;
  outline: none;
  z-index: 2;
  overflow: hidden;
  &:hover {
    background: $c-white;
    &::after {
      content: "";
      height: 12px;
      width: 12px;
      position: absolute;
      display: inline-block;
      color: rgba(55, 56, 58, 0.4);
      text-align: center;
      background-image: url("/images/check-hover.svg");
      background-repeat: no-repeat;
      background-position: 1px 1px;
    }
  }
  &:checked {
    border-color: $c-green;
    &::before {
      content: "";
      height: 12px;
      width: 12px;
      position: absolute;
      display: inline-block;
      color: rgba(55, 56, 58, 0.4);
      text-align: center;
      background: $c-green;
    }
    &::after {
      content: "";
      height: 12px;
      width: 12px;
      position: absolute;
      display: inline-block;
      color: rgba(55, 56, 58, 0.4);
      text-align: center;
      background-image: url("/images/check.svg");
      background-repeat: no-repeat;
      background-position: 1px 1px;
    }
  }

  &::after {
    content: "";
    display: block;
    position: relative;
    z-index: 1;
  }
}
</style>
