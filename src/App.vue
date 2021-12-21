<template>
  <div id="app">
    <table align=center>
    <tr>
        <td colspan=2><Navigation @NavClicked="NavigationClicked"/></td>
    </tr>
    <tr>
     <td><CharacterList @CharacterListClicked="CharacterChange"/></td>
     <td>
        <CharacterCreate @Generate="CreateClicked" v-bind:show_create="show_create"/>
        <CharacterDetail v-bind:selected_character="character_detail" v-bind:show_detail="show_detail"></CharacterDetail>
    </td>
    </tr>
    </table>
  </div>
</template>

<script>
import CharacterList from './components/CharacterList.vue';
import CharacterDetail from './components/CharacterDetail.vue';
import CharacterCreate from './components/CharacterCreate.vue';
import Navigation from './components/Navigation.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CharacterList,
    CharacterDetail,
    CharacterCreate,
    Navigation
  },
  data() {
    return {
        character_detail: '',
        show_detail: true,
        show_create: false,
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
        },
        NavigationClicked(event) {
            switch(event.target.id) {
                case 'view_characters':
                    this.show_detail = true;
                    this.show_create = false;
                    break;
                case 'create_character':
                    this.show_detail = false;
                    this.show_create = true;
                    break;
            }
        },
        CreateClicked(event) {
            this.character_detail = event;
            this.show_detail = true;
            this.show_create = false;
        }
  }
}
</script>
