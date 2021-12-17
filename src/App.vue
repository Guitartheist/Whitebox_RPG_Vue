<template>
  <div id="app">
    <table align=center><CharacterList/></table>
  </div>
</template>

<script>
import CharacterList from './components/CharacterList.vue';

export default {
  name: 'App',
  components: {
    CharacterList,
  }
}
</script>
