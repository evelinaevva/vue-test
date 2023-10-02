<template>
  <div>
    <table>
      <tbody>
        <tr>
          <td>
            <button type="button" @click="sortUsers('name')">
              <span>Имя</span><img src="../../src/ui/sort.png" alt="sort" />
            </button>
          </td>
          <td>
            <button type="button" @click="sortUsers('phone')">
              <span>Телефон</span><img src="../../src/ui/sort.png" alt="sort" />
            </button>
          </td>
        </tr>
        <User v-for="user in users" :user="user" :key="user.id" />
      </tbody>
    </table>
  </div>
</template>

<script>
import User from './User';

export default {
  name: 'Users',
  components: {
    User,
  },
  props: {
    users: {
      type: Array,
      required: true,
    },
  },
  methods: {
    sortUsers(key) {
      this.$emit('sortUsers', key);
    },
  },
};
</script>

<style scoped>
table {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column nowrap;
  border-collapse: collapse;
}
button {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-flow: row nowrap;
  width: 9rem;
  height: 2rem;
  color: #474766;
  background-color: white;
}

button:hover {
  color: #2589ff;
  background-color: #f4f9ff;
}
</style>
