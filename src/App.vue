<template>
  <div id="app">
    <table align=center>
    <tr>
     <td><CharacterList @CharacterListClicked="CharacterChange"/></td>
     <td><CharacterDetail v-bind:selected_character="character_detail"></CharacterDetail></td>
    </tr>
    </table>
  </div>
</template>

<script>
import CharacterList from './components/CharacterList.vue';
import CharacterDetail from './components/CharacterDetail.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CharacterList,
    CharacterDetail
  },
  data() {
    return {
        character_detail: ''
    }
  },
  methods: {
    CharacterChange(event) {
        this.fetch_id = event.target.id;
        axios.get(`http://127.0.0.1:8000/whitebox/characters/` + this.fetch_id + '/')
    .then(response => {
      // JSON responses are automatically parsed.
      this.character_detail = response.data;
    })
    .catch(e => {
      this.errors.push(e)
    })
    }
  }
}
</script>
