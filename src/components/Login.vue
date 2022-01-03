<template>
  <div class=LoginVue v-if="show_login=='login'">
  <h1>Login</h1>
  <table>
   <tr><td>Username</td><td align=right><input type=text v-model="username"></td></tr>
   <tr><td>Password</td><td align=right><input type=password v-model="password" @keyup.enter="Login"></td></tr>
   <tr><td colspan=2 align=left><input type=submit value=Login @click="Login"></td></tr>
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
      errors: ''
    }
  },

  props: {
    show_login : String
  },

  created() {
  },

  methods: {
    async Login() {
        await HTTP.post('signin',
        {
            username: this.username,
            password: this.password
        }).then()
        .catch(e => {
          this.errors.push(e)
        });
        this.$emit('UserLogon');
    }
  }
}
</script>