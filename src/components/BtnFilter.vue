<script>
export default {
  emits: ["filter"],
  data() {
    return {
      isClicked: false,
    };
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    color: {
      type: String,
      required: true,
    },
  },
  methods: {
    onClick() {
      this.isClicked = !this.isClicked;
      this.$emit("filter", this.title);
    },
  },
};
</script>

<template>
  <button
    class="ea-btn"
    v-bind:style="{ 'background-color': color }"
    :class="{ 'btn--clicked': isClicked }"
    @click="onClick"
  >
    Equipe {{ title }}
  </button>
</template>

<style scoped>
.btn--1 {
  background-color: #0080ff;
}

.btn--2 {
  background-color: #e78721;
}

.btn--3 {
  background-color: #47da5f;
}

.btn--clicked {
  background-color: #636363 !important;
}
</style>
