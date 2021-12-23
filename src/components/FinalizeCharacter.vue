<template>
  <div class=CharacterDetail v-if="show_finalize=='finalize'">
      <h1 v-if="selected_character">{{ selected_character.name }} the {{ selected_character.character_role }}</h1>
      <select v-model="c_role">
            <option value=0>Fighter (Strength)</option>
            <option value=1>Mage (Intelligence)</option>
            <option value=2>Thief (Dexterity)</option>
            <option value=3>Cleric (Wisdom)</option>
      </select>
      <table v-if="selected_character">
      <DumpStat v-bind:stat_name="s" v-bind:original_value="selected_character.strength" v-bind:dump_value="dump_s" v-bind:new_value="new_s" @Plus="Plus" @Minus="Minus" />
      <DumpStat v-bind:stat_name="d" v-bind:original_value="selected_character.dexterity" v-bind:dump_value="dump_d" v-bind:new_value="new_d" @Plus="Plus" @Minus="Minus"/>
      <DumpStat v-bind:stat_name="co" v-bind:original_value="selected_character.constitution" v-bind:dump_value="dump_co" v-bind:new_value="new_co" @Plus="Plus" @Minus="Minus"/>
      <DumpStat v-bind:stat_name="i" v-bind:original_value="selected_character.intelligence" v-bind:dump_value="dump_i" v-bind:new_value="new_i" @Plus="Plus" @Minus="Minus"/>
      <DumpStat v-bind:stat_name="w" v-bind:original_value="selected_character.wisdom" v-bind:dump_value="dump_w" v-bind:new_value="new_w" @Plus="Plus" @Minus="Minus"/>
      <DumpStat v-bind:stat_name="ch" v-bind:original_value="selected_character.charisma" v-bind:dump_value="dump_ch" v-bind:new_value="new_ch" @Plus="Plus" @Minus="Minus"/>
      </table>
      <input type=submit value=Finalize @click="Finalize">
  </div>
</template>

<script>
import DumpStat from './DumpStat.vue';
import { HTTP } from './http-common';

export default {
    components: {
   DumpStat
  },
  data() {
    return {
        s: 'Strength',
        d: 'Dexterity',
        co: 'Constitution',
        i: 'Intelligence',
        w: 'Wisdom',
        ch: 'Charisma',
        dump_s: 0,
        dump_d: 0,
        dump_co: 0,
        dump_i: 0,
        dump_w: 0,
        dump_ch: 0,
        new_s: 0,
        new_d: 0,
        new_co: 0,
        new_i: 0,
        new_w: 0,
        new_ch: 0,
        c_role: 0,
      errors: []
    }
  },

  props: {
    selected_character : Object,
    show_finalize : String
  },

  computed: {
    DumpTotal() {
        return this.dump_s + this.dump_d + this.dump_co + this.dump_i + this.dump_w + this.dump_ch;
    }
  },

  created() {
  },

  methods: {
    Plus(event) {
        switch (event) {
            case this.s:
                this.dump_s++;
                this.new_s = this.selected_character.strength - this.dump_s;
            break;

            case this.d:
                this.dump_d++;
                this.new_d = this.selected_character.dexterity - this.dump_d;
            break;

            case this.co:
                this.dump_co++;
                this.new_co = this.selected_character.constitution - this.dump_co;
            break;

            case this.i:
                this.dump_i++;
                this.new_i = this.selected_character.intelligence - this.dump_i;
            break;

            case this.w:
                this.dump_w++;
                this.new_w = this.selected_character.wisdom - this.dump_w;
            break;

            case this.ch:
                this.dump_ch++;
                this.new_ch = this.selected_character.charisma - this.dump_ch;
            break;
        }
        this.NewCoreValue();
    },
    Minus(event) {
        switch (event) {
            case this.s:
                this.dump_s--;
                this.new_s = this.selected_character.strength - this.dump_s;
            break;

            case this.d:
                this.dump_d--;
                this.new_d = this.selected_character.dexterity - this.dump_d;
            break;

            case this.co:
                this.dump_co--;
                this.new_co = this.selected_character.constitution - this.dump_co;
            break;

            case this.i:
                this.dump_i--;
                this.new_i = this.selected_character.intelligence - this.dump_i;
            break;

            case this.w:
                this.dump_w--;
                this.new_w = this.selected_character.wisdom - this.dump_w;
            break;

            case this.ch:
                this.dump_ch--;
                this.new_ch = this.selected_character.charisma - this.dump_ch;
            break;
        }
        this.NewCoreValue();
    },
    NewCoreValue() {
        switch (this.c_role) {
            case 0:
                this.new_s = this.selected_character.strength + this.DumpTotal - this.dump_s;
                break;
            case 1:
                this.new_i = this.selected_character.intelligence +  this.DumpTotal - this.dump_i;
                break;
            case 2:
                this.new_d = this.selected_character.dexterity + this.DumpTotal - this.dump_d;
                break;
            case 3:
                this.new_w = this.selected_character.wisdom - this.dump_w;
                break;
        }
    },
    Finalize() {
        HTTP.put('characters/'+this.selected_character.id+'/',
        {
            c_role: this.c_role,
            strength: this.dump_s,
            dexterity: this.dump_d,
            constitution: this.dump_co,
            intelligence: this.dump_i,
            wisdom: this.dump_w,
            charisma: this.dump_ch
        }).then()
        .catch(e => {
          this.errors.push(e)
        })
        this.$emit('CharacterFinalized',this.selected_character.id);
    }
  }
}
</script>