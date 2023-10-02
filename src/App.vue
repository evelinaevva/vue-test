<template>
  <div id="app">
    <h1>Test</h1>
    <Register
      :users="users"
      @addUser="addUser"
      v-if="isShowing"
      @closeModal="toggleModal"
    />
    <Users :users="users" @sortUsers="sortUsers" />
    <Add @showRegister="toggleModal" />
  </div>
</template>

<script>
import Users from './components/Users';
import Register from './components/Register';
import Add from './components/Add';

export default {
  name: 'App',
  components: { Register, Users, Add },
  data: () => ({
    users: [
      {
        id: 1,
        name: 'Марина',
        phone: '+7 941 123 2142',
        boss: null,
      },
      {
        id: 2,
        name: 'Петр',
        phone: '+7 941 123 2142',
        boss: null,
      },
      {
        id: 3,
        name: 'Алексей',
        phone: '+7 941 123 2142',
        boss: null,
      },
      {
        id: 4,
        name: 'Иван',
        phone: '+7 941 123 2142',
        boss: 3,
      },
      {
        id: 5,
        name: 'Борис',
        phone: '+7 941 123 2142',
        boss: null,
        children: [{
          id: 6,
          name: 'Ребенок',
          phone: '+7 987 138 7691',
          boss: 3,
        }],
      },
    ],
    isShowing: false,
    order: null,
  }),
  methods: {
    addUser(user) {
      this.users.push(user);
    },
    sortUsers(key = 'name') {
      if (this.order === null || this.order === 'asc') {
        this.users.sort((a, b) => {
          if (a[key] > b[key]) {
            return 1;
          }
          if (a[key] < b[key]) {
            return -1;
          }
          return 0;
        });
        this.order = 'desc';
      } else {
        this.users.sort((a, b) => {
          if (a[key] > b[key]) {
            return -1;
          }
          if (a[key] < b[key]) {
            return 1;
          }
          return 0;
        });
        this.order = 'asc';
      }
    },
    transform(users) {
      const map = {};
      const result = {};

      // eslint-disable-next-line arrow-parens
      users.forEach(user => {
        map[user.id] = user;
        if (user.boss === null) {
          result[user.id] = map[user.id];
        }
      });

      // eslint-disable-next-line arrow-parens
      users.forEach(user => {
        if (user.boss !== null) {
          const parent = map[user.boss];
          if (!parent.employees) {
            parent.employees = {};
          }
          parent.employees[user.id] = map[user.id];
        }
      });

      return result;
    },
    toggleModal() {
      this.isShowing = !this.isShowing;
    },
  },
  mounted() {
    if (localStorage.getItem('users')) {
      try {
        this.users = JSON.parse(localStorage.getItem('users'));
      } catch (e) {
        localStorage.removeItem('users');
      }
    }
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #474766;
}

button {
  background-color: #2589ff;
  color: white;
  padding: 1rem 1.5rem;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
  margin: 1rem;
}

button:hover {
  background-color: #489cff;
}

img {
  max-width: 2rem;
}

h1 {
  margin: 1rem;
}

span {
  margin-right: 1rem;
}
</style>
