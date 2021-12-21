<template>
  <div v-if="show_create" class=CharacterCreate border=1>
      <h1>Generate a new character</h1>
      <label>Name: </label>
      <input type=text v-model="name">
      <input type=submit value="Generate" @click="Generate">
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
        name: 'Scrub',
        errors: []
    }
  },

  props: {
    show_create : Boolean
  },

  created() {
  },

  methods: {
    Generate(event) {
        axios.get(`http://127.0.0.1:8000/whitebox/character_generate/` + this.name + '/')
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