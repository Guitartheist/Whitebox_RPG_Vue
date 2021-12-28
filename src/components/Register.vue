<template>
  <div class=RegisterVue v-if="show_register=='register'">
  <h1>Register</h1>
  <table>
    <tr><td>Email</td><td align=right><input type=text v-model="email"></td></tr>
   <tr><td>Username</td><td align=right><input type=text v-model="username"></td></tr>
   <tr><td>Password</td><td align=right><input type=password v-model="password"></td></tr>
   <tr><td colspan=2 align=left><input type=submit value=Login @click="Register"></td></tr>
   {{ errors }}
   </table>
  </div>
</template>

<script>
import { HTTP } from './http-common';

export default {
  data() {
    return {
        username: '',
        password: '',
        email: '',
      errors: ''
    }
  },

  props: {
    show_register : String
  },

  created() {
  },

  methods: {
    Register() {
        HTTP.post('register',
        {
            username: this.username,
            password: this.password,
            email: this.email
        }).then()
        .catch(e => {
          this.errors.push(e)
        })
        this.$emit('UserRegistered');
    }
  }
}
</script>