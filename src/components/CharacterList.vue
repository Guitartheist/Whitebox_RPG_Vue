<template>
  <div class=CharacterList v-if="show_list=='character_list'">
  <h1>Character List</h1>
  <ol>
    <li v-for="character in characters.results" v-bind:key="character.id" :id="character.id" @click="NameClicked">
      {{ character.name }} the {{ character.character_role }}
    </li>
  </ol>
  <h3 @click="Next" class=Navigation align=center v-if="characters.next">Older</h3>
  <h3 @click="Previous" class=Navigation align=center v-if="characters.previous">Newer</h3>
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
    characters : Object
  },

  methods: {
    NameClicked(event) {
        this.$emit('CharacterListClicked', event.target.id);
    },
    Next() {
        this.$emit('CharacterListPageNext', this.characters.next);
    },
    Previous() {
        this.$emit('CharacterListPagePrevious', this.characters.previous);
    }
  }
}
</script>