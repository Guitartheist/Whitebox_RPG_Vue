<template>
  <div class=CharacterList v-if="show_list=='character_list'">
  <ol>
    <li v-for="character in characters" v-bind:key="character.id" :id="character.id" @click="NameClicked">
      {{ character.name }} the {{ character.character_role }}
    </li>
  </ol>
  </div>
</template>

<script>
import { HTTP } from './http-common';

export default {
  data() {
    return {
      characters: [],
      errors: []
    }
  },

  props: {
    show_list : String,
    update : Boolean,
    url : String
  },

  // Fetches character list when the component is created.
  created() {
    HTTP.get(this.url)
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
        this.$emit('CharacterListClicked', event.target.id);
    },
    UpdateList() {
        HTTP.get(this.url)
        .then(response => {
          // JSON responses are automatically parsed.
          this.characters = response.data
        })
        .catch(e => {
          this.errors.push(e)
        })
        this.update = false;
    }
  },

  watch: {
    url (val, oldVal) {
        if (val!=oldVal) {
            this.UpdateList();
        }
    },
    update (val, oldVal) {
        if (val!=oldVal) {
            this.update = false;
            this.UpdateList();
        }
    }
  }
}
</script>