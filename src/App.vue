<template>
  <div id="app">
    <table align=center>
    <tr>
        <td colspan=2><Navigation @NavClicked="NavigationClicked"
        v-bind:update="update_navigation"/></td>
    </tr>
    <tr>
     <td>
         <CharacterList @CharacterListClicked="CharacterChange"
          v-bind:url="character_list_url"
          v-bind:show_list="show_left"
          v-bind:update="update_character_list"
          ref="character_list"/>
     </td>
     <td>
        <CharacterCreate @Generate="CreateClicked" v-bind:show_create="show_right"/>
        <CharacterDetail
            v-bind:selected_character="character_detail"
            v-bind:show_detail="show_right"
            @DeleteClicked="DeleteClicked"
            @FinalizeClicked="FinalizeClicked"
        />
        <FinalizeCharacter
            v-bind:selected_character="character_detail"
            v-bind:show_finalize="show_right"
            @CharacterFinalized="CharacterChange"
        />
        <Login v-bind:show_login="show_right" @UserLogon="UserLogon"/>
        <Register v-bind:show_register="show_right" @UserRegistered="UserRegistered"/>
    </td>
    </tr>
    </table>
  </div>
</template>

<script>
import CharacterList from './components/CharacterList.vue';
import CharacterDetail from './components/CharacterDetail.vue';
import FinalizeCharacter from './components/FinalizeCharacter.vue';
import CharacterCreate from './components/CharacterCreate.vue';
import Navigation from './components/Navigation.vue';
import Login from './components/Login.vue';
import Register from './components/Register.vue';
import { HTTP } from './components/http-common';

export default {
  name: 'App',
  components: {
    CharacterList,
    CharacterDetail,
    FinalizeCharacter,
    CharacterCreate,
    Navigation,
    Login,
    Register
  },
  data() {
    return {
        character_detail: '',
        show_left: 'character_list',
        show_right: 'detail',
        character_list_url: 'character_list',
        update_character_list: false,
        update_navigation: false
    }
  },
  created () {
    HTTP.get('get_username')
            .then(response => {
              // JSON responses are automatically parsed.
              this.username = response.data.username;
            })
            .catch(e => {
              this.errors.push(e)
            })
  },
  methods: {
        CharacterChange(event) {
                this.fetch_id = event.target.id;
                HTTP.get(`characters/` + this.fetch_id + '/')
            .then(response => {
              // JSON responses are automatically parsed.
              this.character_detail = response.data;
              this.show_right = 'detail';
            })
            .catch(e => {
              this.errors.push(e)
            })
        },
        NavigationClicked(event) {
            switch(event.target.id) {
                case 'view_characters':
                    this.character_list_url = 'character_list';
                    this.show_left = 'character_list';
                    this.show_right = 'detail';
                    break;
                case 'create_character':
                    this.show_right = 'create';
                    break;
                case 'my_characters':
                    this.character_list_url = 'my_character_list';
                    this.show_left = 'character_list';
                    this.show_right = 'detail';
                    break;
                case 'register':
                    this.show_right = 'register';
                    break;
                case 'login':
                    this.show_right = 'login';
                    break;
                case 'logout':
                    this.show_right = 'login';
                     HTTP.get('LogoutPage')
                        .then()
                        .catch(e => {
                          this.errors.push(e)
                        });
                    break;
            }
            this.update_navigation = true;
        },
        CreateClicked(event) {
            this.character_detail = event;
            this.show_left = 'character_list';
            this.show_right = 'detail';
            this.update_character_list = true;
        },
        FinalizeClicked() {
            this.show_right = 'finalize';
        },
        async DeleteClicked() {
            await HTTP.delete(`characters/` + this.fetch_id + '/')
            .then(response => {
              // JSON responses are automatically parsed.
              this.character_detail = response.data;
              this.show_right = 'detail';
            })
            .catch(e => {
              this.errors.push(e)
            })
            this.update_character_list = true;
            this.show_right = 'create';
        },
        UserRegistered() {
            this.show_right = 'login';
        },
        UserLogon() {
            this.show_right = 'detail';
            this.update_navigation = true;
        }
  }
}
</script>
