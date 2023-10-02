<template>
    <tr>
      <td>{{ user.name }}</td>
      <td>{{ user.phone }}</td>
    </tr>
</template>

<script>
export default {
  name: 'User',
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
th {
  font-weight: normal;
  color: #ffffff;
  background-color: #354251;
  font-size: 13px;
}
td,
th {
  padding: 10px 15px;
  border: 1px solid gainsboro;
  background-color: #ffffff;
}
</style>
