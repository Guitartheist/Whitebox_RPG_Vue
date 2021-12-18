<template>
  <div align=left  v-if="selected_character">
      <h1>{{ selected_character.name }} the {{ selected_character.character_role }}</h1>
      <table>
      <tr><td>Strength</td><td><b>{{ selected_character.strength }}</b></td></tr>
      <tr><td>Dexterity</td><td><b>{{ selected_character.dexterity }}</b></td></tr>
      <tr><td>Constitution</td><td><b>{{ selected_character.constitution }}</b></td></tr>
      <tr><td>Intelligence</td><td><b>{{ selected_character.intelligence }}</b></td></tr>
      <tr><td>Wisdom</td><td><b>{{ selected_character.wisdom }}</b></td></tr>
      <tr><td>Charisma</td><td><b>{{ selected_character.charisma }}</b></td></tr>
      <tr><td>Gold</td><td></td><b>{{ selected_character.gold }}</b></tr>
      <tr><td>Silver</td><td><b>{{ selected_character.silver }}</b></td></tr>
      <tr><td>Copper</td><td><b>{{ selected_character.copper }}</b></td></tr>
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

  props:
  { selected_character : Object},

  created() {
  }
}
</script>