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

export default {
  data() {
    return {
      errors: []
    }
  },

  props: {
    show_list : String,
    characters : Array
  },

  // Fetches character list when the component is created.
  created() {
  },

  methods: {
    NameClicked(event) {
        this.$emit('CharacterListClicked', event.target.id);
    }
  }
}
</script>