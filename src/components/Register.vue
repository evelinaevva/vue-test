<template>
  <div class="dialog" @click="$emit('closeModal')">
    <div class="register" @click.stop>
      <button type="button" class="closeButton" @click="$emit('closeModal')">
        <img src="../../src/ui/close.png" alt="close" />
      </button>
      <div c>Добавление нового сотрудника</div>
      <Error :error="error" v-if="error" />
      <form>
        <label
          >Имя:<br />
          <input type="text" name="name" v-model="name" required/></label
        ><br />
        <label
          >Телефон: +7 <br />
          <input
            type="tel"
            name="phone"
            v-model="phone"
            class="input-tel"
            :class="{
              notvalid: error,
              valid: error === false,
            }"
            @input="phoneValidator"
            maxlength="10"
            minlength="10"
            placeholder="9001234567"
            required
        /></label>
        <br />
        <label
          >Начальник:<br />
          <select name="boss" v-model="boss">
            <option value="null"></option>
            <option v-for="user in users" :value="user" :key="user.id">{{
              user.name
            }}</option>
          </select>
        </label>
        <button type="submit" @click.prevent="addUser">Сохранить</button>
      </form>
    </div>
  </div>
</template>

<script>

import Error from './Error';

export default {
  name: 'Register',
  components: {
    Error,
  },
  props: {
    users: {
      type: Array,
      required: true,
    },
  },
  data: () => ({
    name: null,
    phone: null,
    boss: null,
    error: null,
    validPhone: null,
  }),
  methods: {
    addUser() {
      if (this.name && this.phone) {
        if (this.phone.length === 10 && /^[\d]{10}$/.test(this.phone)) {
          const user = {
            id: Date.now(),
            name: this.name[0].toUpperCase() + this.name.slice(1).toLowerCase(),
            phone: this.validPhone,
            boss: this.boss,
          };
          this.$emit('addUser', user);
          this.saveUsers();
          this.$emit('closeModal');
        } else {
          this.error = 'Введите номер телефона';
        }
      } else {
        this.error = 'Заполните поля';
      }
    },
    saveUsers() {
      const parsed = JSON.stringify(this.users);
      localStorage.setItem('users', parsed);
    },
    phoneValidator() {
      if (/^[\d]{10}$/.test(this.phone)) {
        this.validPhone = `+7 ${this.phone.slice(0, 3)} ${this.phone.slice(
          3,
          6,
        )} ${this.phone.slice(6, 10)}`;
      } else if (this.phone.length === 10) {
        this.error = 'Введите номер телефона';
      }
    },
  },
};
</script>

<style scoped>
.dialog {
  margin: 0;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
}

.dialog,
.register,
form {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column nowrap;
}

.register {
  padding: 0.6rem;
  background: white;
  border: 0.1rem solid #ccc;
  border-radius: 1rem;
  box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
}

input,
select,
form {
  padding: 1rem;
}

input,
select {
  margin: 0.6rem 0;
  border: 1px solid gainsboro;
  border-radius: 0.5rem;
}

form {
  margin: 0 5rem;
}
.closeButton {
  all: unset;
  cursor: pointer;
  display: flex;
  align-self: flex-end;
}

label {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-flow: column nowrap;
}
</style>
