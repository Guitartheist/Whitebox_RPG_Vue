<template>
  <div>
    <table align=center class=Navigation>
    <tr>
    <td id="view_characters" class=Navigation @click="NavClicked">View Characters</td>
    <td id="create_character" class=Navigation @click="NavClicked">Create Character</td>
    <td id="my_characters" class=Navigation @click="NavClicked">My Characters</td>
    <td id="auth" class=Navigation @click="NavClicked">Auth</td>
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

  // Fetches character list when the component is created.
  created() {
  },

  methods: {
    NavClicked(event) {
        this.$emit('NavClicked', event);
    }
  }
}
</script>