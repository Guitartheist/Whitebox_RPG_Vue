<template>
  <div>
    <table align=center class=Navigation>
    <tr>
    <td id="view_characters" class=Navigation @click="NavClicked">View Characters</td>
    <td id="create_character" class=Navigation @click="NavClicked">Create Character</td>
    <td id="my_characters" class=Navigation @click="NavClicked" v-if="username">My Characters</td>
    <td id="register" class=Navigation @click="NavClicked" v-if="!username">Register</td>
    <td id="login" class=Navigation @click="NavClicked" v-if="!username">Login</td>
    <td id="logout" class=Navigation @click="NavClicked" v-if="username">Logout {{ username }}</td>
    </tr>
    </table>
  </div>
</template>

<script>
import { HTTP } from './http-common';

export default {
  data() {
    return {
      username: '',
      errors: []
    }
  },

  props: {
    update : Boolean
  },

  // Fetches character list when the component is created.
  created() {
    this.UserChange();
  },

  methods: {
    UserChange() {
            HTTP.get('get_username')
            .then(response => {
              // JSON responses are automatically parsed.
              this.username = response.data.username;
            })
            .catch(e => {
              this.errors.push(e)
            })
        },
    NavClicked(event) {
        this.$emit('NavClicked', event);
    }
  },

  watch: {
    update (val, oldVal) {
        if (val!=oldVal && val==true) {
            this.update = false;
            this.UserChange();
        }
    }
  }
}
</script>