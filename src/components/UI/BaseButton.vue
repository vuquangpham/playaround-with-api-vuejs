<template>
  <button @click="handleButtonClick">
    <slot></slot>
  </button>
</template>

<script>
export default {
  emits: ["click"],
  methods: {
    handleButtonClick() {
      this.$emit("click");
    },
  },
};
</script>

<style scoped>
button {
  font: inherit;
  border: 1px solid #360032;
  background-color: #360032;
  color: white;
  padding: 0.5rem 2rem;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #5c0556;
  border-color: #5c0556;
}
</style>
