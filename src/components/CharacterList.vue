<template>
  <div align=left>
  <ol>
    <li v-for="character in characters" v-bind:key="character.id" :id="character.id" @click="NameClicked">
      {{ character.name }} the {{ character.character_role }}
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

  // Fetches character list when the component is created.
  created() {
    axios.get(`http://127.0.0.1:8000/whitebox/character_list`)
    .then(response => {
      // JSON responses are automatically parsed.
      this.characters = response.data
    })
    .catch(e => {
      this.errors.push(e)
    })
  },

  methods: {
    NameClicked(event) {
        this.$emit('CharacterListClicked', event);
    }
  }
}
</script>