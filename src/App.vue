<template>
  <div id="app">
    <table align=center>
    <tr>
        <td colspan=2><Navigation @NavClicked="NavigationClicked"
        v-bind:username="username"/></td>
    </tr>
    <tr>
     <td>
         <CharacterList @CharacterListClicked="CharacterChange"
         @CharacterListPageNext="CharacterListPageNext"
         @CharacterListPagePrevious="CharacterListPagePrevious"
          v-bind:show_list="show_left"
          v-bind:characters="characters"
          />
     </td>
     <td>
        <CharacterCreate @Generate="CreateClicked" v-bind:show_create="show_right"/>
        <CharacterDetail
            v-bind:selected_character="character_detail"
            v-bind:show_detail="show_right"
            v-bind:username="username"
            @DeleteClicked="DeleteClicked"
            @FinalizeClicked="FinalizeClicked"
        />
        <FinalizeCharacter
            v-bind:selected_character="character_detail"
            v-bind:show_finalize="show_right"
            v-bind:username="username"
            @CharacterFinalized="CharacterFinalized"
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
        characters: [],
        character_detail: '',
        show_left: 'character_list',
        show_right: 'detail',
        character_list_url: 'CharacterList',
        username: '',
        list_page: 1
    }
  },
  created () {
    this.UserChange();
    this.UpdateList();
  },
  methods: {
        async CharacterChange(id) {
                this.fetch_id = id;
                await HTTP.get(`characters/` + this.fetch_id + '/')
            .then(response => {
              // JSON responses are automatically parsed.
              this.character_detail = response.data;
              this.show_right = 'detail';
            })
            .catch(e => {
              this.errors.push(e)
            })
            this.show_right = 'detail';
        },
        CharacterFinalized(id) {
            this.CharacterChange(id);
            this.UpdateList();
        },
        async NavigationClicked(event) {
            switch(event.target.id) {
                case 'view_characters':
                    this.list_page = 1;
                    this.character_list_url = 'CharacterList';
                    this.UpdateList();
                    this.show_left = 'character_list';
                    this.show_right = 'detail';
                    break;
                case 'create_character':
                    this.show_right = 'create';
                    break;
                case 'my_characters':
                    this.list_page = 1;
                    this.character_list_url = 'MyCharacterList';
                    this.show_left = 'character_list';
                    this.UpdateList();
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
                    await HTTP.get('LogoutPage')
                        .then()
                        .catch(e => {
                          this.errors.push(e)
                        });
                    this.UserChange();
                    break;
            }
        },
        CharacterListPageNext(url) {
            this.list_page += 1;
            HTTP({ url: url, baseURL: '' })
            .then(response => {
              // JSON responses are automatically parsed.
              this.characters = response.data
            })
            .catch(e => {
              this.errors.push(e)
            })
        },
        CharacterListPagePrevious(url) {
            this.list_page -= 1;
            HTTP({ url: url, baseURL: '' })
            .then(response => {
              // JSON responses are automatically parsed.
              this.characters = response.data
            })
            .catch(e => {
              this.errors.push(e)
            })
        },
        CreateClicked(event) {
            this.character_detail = event;
            this.show_left = 'character_list';
            this.show_right = 'detail';
            this.UpdateList();
        },
        FinalizeClicked() {
            this.show_right = 'finalize';
        },
        UserChange() {
            HTTP.get('get_username')
            .then(response => {
              // JSON responses are automatically parsed.
              this.username = response.data.username;
            })
            .catch(e => {
              this.errors.push(e)
            })
        },
        UpdateList() {
            HTTP.get(this.character_list_url + "?page=" + this.list_page)
            .then(response => {
              // JSON responses are automatically parsed.
              this.characters = response.data
            })
            .catch(e => {
              this.errors.push(e)
            })
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
            this.UpdateList();
            this.show_right = 'create';
        },
        UserRegistered() {
            this.show_right = 'login';
        },
        UserLogon() {
            this.show_right = 'detail';
            this.UserChange();
        }
  }
}
</script>
