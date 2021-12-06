<template>
  <figure class="airline-logo">
    <div class="airline-logo__image">
      <img :src="src" :alt="name" />
    </div>
    <figcaption class="airline-logo__figcaption">{{ name }}</figcaption>
  </figure>
</template>

<script>
export default {
  name: "AirlineLogo",
  props: {
    name: {
      type: String,
      default: "",
    },
    src: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
.airline-logo {
  margin: 0;
  display: flex;
  align-items: center;
  background-color: $c-white;
  max-width: 320px;
  border-radius: 4px;

  &__image {
    overflow: hidden;
    margin-right: 12px;
    display: flex;
    align-items: center;
    img {
      object-fit: contain;
      width: 24px;
      height: 24px;
    }
  }
  &__figcaption {
    font-family: $f-primary;
    font-weight: 600;
    width: 100px;
  }
}
</style>
