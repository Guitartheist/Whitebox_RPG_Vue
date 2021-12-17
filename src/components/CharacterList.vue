<template>
  <div>
  <ol>
    <li v-for="character in characters" v-bind:key="character">
      {{ character.name }}
    </li>
  </ol>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      characters: [],
      errors: []
    }
  },

  // Fetches posts when the component is created.
  created() {
    axios.get(`http://127.0.0.1:8000/whitebox/character_list`)
    .then(response => {
      // JSON responses are automatically parsed.
      this.characters = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  }
}
</script>