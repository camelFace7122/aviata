<template>
  <div class="filters">
    <div class="filters__head">
      <h4>Опции тарифа</h4>
      <CloseFilterIcon class="filters__close-icon" @click.native="reset" />
    </div>
    <ul>
      <li v-for="option in options" :key="option[0]">
        <Checkbox v-model="model" :name="name" :value="option[0]">{{
          option[1]
        }}</Checkbox>
      </li>
    </ul>
  </div>
</template>

<script>
import Checkbox from "@/components/atoms/Checkbox";
import CloseFilterIcon from "@/components/atoms/CloseFilterIcon";

export default {
  emits: ["select", "reset"],
  components: { Checkbox, CloseFilterIcon },
  model: {
    prop: "checked",
    event: "change",
  },
  props: {
    options: {
      type: Array,
      default: () => [],
    },
    checked: {
      type: Array,
      default: () => [],
    },
    name: {
      type: String,
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
  methods: {
    reset() {
      this.$emit("reset");
    },
  },
};
</script>

<style lang="scss" scoped>
.filters {
  background-color: $c-bg-light;
  border-radius: 4px;
  max-width: 320px;
  min-width: 240px;

  &__close-icon {
    color: #c4c4c4;
    transition: color 0.2s linear;
    cursor: pointer;
    &:hover {
      color: $c-dark-blue;
    }
  }

  &__head {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px;
    h4 {
      font-size: 14px;
      line-height: 18px;
      font-weight: 700;
      margin: 0;
    }
  }

  ul {
    list-style: none;
    padding-left: 0;
    margin: 0;
    padding-bottom: 12px;
    max-height: 254px;
    overflow: auto;
    li {
      label {
        transition: background-color 0.2s linear;
        &:hover {
          background-color: #ebebeb;
          cursor: pointer;
        }
        padding: 10px 12px;
      }
    }
  }
}
</style>
