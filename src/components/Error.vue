<template>
  <div>
    {{ error }}
  </div>
</template>

<script>
export default {
  name: 'Error',
  props: {
    error: {
      type: String,
    },
  },
};
</script>

<style scoped>
div {
  color: red;
  font-weight: 400;
}
</style>
