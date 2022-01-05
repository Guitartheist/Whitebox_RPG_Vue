<template>
  <div class=CharacterDetail v-if="show_detail=='detail'">
      <h1 v-if="!selected_character">Character Details</h1>
      <h1 v-if="selected_character">{{ selected_character.name }} the {{ selected_character.character_role }}</h1>
      <table v-if="selected_character">
      <tr><td>Strength</td><td class=CharacterStatistic><b>{{ selected_character.strength }}</b></td></tr>
      <tr><td>Dexterity</td><td class=CharacterStatistic><b>{{ selected_character.dexterity }}</b></td></tr>
      <tr><td>Constitution</td><td class=CharacterStatistic><b>{{ selected_character.constitution }}</b></td></tr>
      <tr><td>Intelligence</td><td class=CharacterStatistic><b>{{ selected_character.intelligence }}</b></td></tr>
      <tr><td>Wisdom</td><td class=CharacterStatistic><b>{{ selected_character.wisdom }}</b></td></tr>
      <tr><td>Charisma</td><td class=CharacterStatistic><b>{{ selected_character.charisma }}</b></td></tr>
      <tr><td>Gold</td><td class=CharacterStatistic><b>{{ selected_character.gold }}</b></td></tr>
      <tr><td>Silver</td><td class=CharacterStatistic><b>{{ selected_character.silver }}</b></td></tr>
      <tr><td>Copper</td><td class=CharacterStatistic><b>{{ selected_character.copper }}</b></td></tr>
      </table>
	
	<MeleeWeaponShop v-if="selected_character && selected_character.finalized==true && selected_character.username==username" 
	v-bind:show_shop="show_shop"
	v-bind:available_gold="selected_character.gold"
	@BuyMeleeWeapon="BuyMeleeWeapon"
	/>
	
	<h2 v-if="show_shop!='melee_weapons' && selected_character && selected_character.finalized==true && selected_character.username==username" @click="BuyMelee">
	Buy Melee Weapons</h2>
	
	<h2 v-if="show_shop=='melee_weapons'" @click="CloseMelee">
	Close Melee Weapons</h2>
	
      <h2 v-if="!delete_prep && selected_character && (selected_character.username==username || selected_character.username=='')"
            @click="DeleteClicked">
       Delete </h2>
       <h2 v-if="delete_prep"
            @click="DeleteCancel">
       Cancel Delete </h2>
      <h2 v-if="delete_prep"
            @click="DeleteConfirmed">
       Confirm Delete </h2>
      <h2 v-if="selected_character && selected_character.finalized==false && (selected_character.username==username || selected_character.username=='')"
            @click="FinalizeClicked">
        Finalize </h2>
  </div>
</template>

<script>
import MeleeWeaponShop from './MeleeWeaponShop.vue';

export default {
	components: {
   MeleeWeaponShop
  },
	
  data() {
    return {
        delete_prep: false,
		show_shop: '',
		melee_weapons: [],
        errors: []
    }
  },

  props: {
    selected_character : Object,
    show_detail : String,
    username: String
  },

  created() {
	
  },

  methods: {
    DeleteClicked() {
        this.delete_prep = true;
    },
    DeleteConfirmed(event) {
        this.delete_prep = false;
        this.$emit('DeleteClicked', event);
    },
    DeleteCancel() {
        this.delete_prep = false;
    },
    FinalizeClicked(event) {
        this.$emit('FinalizeClicked', event);
    },
	BuyMelee() {
		this.show_shop = 'melee_weapons';
	},
	CloseMelee() {
		this.show_shop = '';
	},
	BuyMeleeWeapon(melee_weapon_id) {
		this.$emit('BuyMeleeWeapon', melee_weapon_id);
	}
  },
  
  watch: {
	selected_character() {
		this.show_shop = '';
		this.delete_prep = false;
	}
  }
}
</script>