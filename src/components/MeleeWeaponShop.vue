<template>
  <div v-if="show_shop == 'melee_weapons'">
  <h1>Melee Weapons Shop</h1>
  <table>
  <tr>
	<td>Name</td>
	<td>Damage</td>
	<td>Weight</td>
	<td>Cost (Gold)</td>
  </tr>
  <tr v-for="weapon in weapons.results" v-bind:key="weapon.id" :id="weapon.id" >
		<td class=ShoppingValue> {{ weapon.name }} </td> 
		<td class=ShoppingValue> {{ weapon.damage }} </td>
		<td class=ShoppingValue> {{ weapon.weight }} </td>
		<td class=ShoppingValue> {{ weapon.cost_gp }} </td>
		<td v-if="available_gold >= weapon.cost_gp" class=ShoppingValue :id="weapon.id" @click="Buy"> Buy </td>
		<td v-if="available_gold < weapon.cost_gp" class=ShoppingValue>  </td>
		</tr>
	</table>
	<h3 @click="Next" class=Navigation align=center v-if="weapons.next">Older</h3>
  <h3 @click="Previous" class=Navigation align=center v-if="weapons.previous">Newer</h3>
  </div>
</template>

<script>
import { HTTP } from './http-common';

export default {
  data() {
    return {
	weapon: '',
	page: 1,
	weapons: [],
    errors: []
    }
  },

	created() {
		HTTP.get('MeleeWeaponList')
            .then(response => {
              // JSON responses are automatically parsed.
              this.weapons = response.data
            })
            .catch(e => {
              this.errors.push(e)
            });
	},

  props: {
    show_shop : String,
	available_gold: Number
  },

  methods: {
    Next() {
        this.page += 1;
    },
    Previous() {
        this.page -= 1;
    },
	Buy(event) {
		this.weapon = this.weapons.results.find(function(w) {
			if(w.id == event.target.id)
				return true;
			});
		if (confirm("Buy " + this.weapon.name + " for " + this.weapon.cost_gp + " gold?")){
			this.$emit('BuyMeleeWeapon', event.target.id);
		}
	}
  },
  
  watch: {
	show_shop() {
		if (this.show_shop=='melee_weapons') {
			HTTP.get('MeleeWeaponList')
				.then(response => {
				// JSON responses are automatically parsed.
				this.weapons = response.data
				})
				.catch(e => {
				this.errors.push(e)
				});
				
		}
		this.page = 1;
	}
  }
}
</script>