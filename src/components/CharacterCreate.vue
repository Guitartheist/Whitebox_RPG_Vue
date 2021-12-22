<template>
  <div v-if="show_create=='create'" class=CharacterCreate border=1>
      <h1>Generate a new character</h1>
      <label>Name: </label>
      <input type=text v-model="name">
      <input type=submit value="Generate" @click="Generate">
  </div>
</template>

<script>
import { HTTP } from './http-common';

export default {
  data() {
    return {
        name: 'Scrub',
        errors: []
    }
  },

  props: {
    show_create : String
  },

  created() {
  },

  methods: {
    Generate(event) {
        HTTP.get(`character_generate/` + this.name + '/')
        .then(response => {
          // JSON responses are automatically parsed.
          event = response.data;
          this.$emit('Generate', event);
        })
        .catch(e => {
          this.errors.push(e)
        })
    }
  }
}
</script>