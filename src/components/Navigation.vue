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
export default {
  data() {
    return {
      errors: []
    }
  },

  props: {
    username : String,
    update : Boolean
  },

  // Fetches character list when the component is created.
  created() {
  },

  methods: {
    NavClicked(event) {
        this.$emit('NavClicked', event);
    }
  },

  watch: {
    username (val, oldVal) {
        if (val!=oldVal) {
            this.username = val;
        }
    },
    update (val, oldVal) {
        if (val!=oldVal) {
            this.update = false;
        }
    }
  }
}
</script>