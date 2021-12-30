<template>
  <div class=CharacterDetail v-if="show_finalize=='finalize'">
      <h1 v-if="selected_character">{{ selected_character.name }} the {{ selected_character.character_role }}</h1>
      <select v-model="c_role" @change="ResetNewValues">
            <option value=0>Fighter (Strength)</option>
            <option value=1>Mage (Intelligence)</option>
            <option value=2>Thief (Dexterity)</option>
            <option value=3>Cleric (Wisdom)</option>
      </select>
      <table v-if="selected_character">
      <DumpStat v-bind:stat_name="s" v-bind:original_value="selected_character.strength" v-bind:dump_value="dump_s" v-bind:new_value="new_s" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="str_pc" v-bind:minus_enabled="str_mc"/>
      <DumpStat v-bind:stat_name="d" v-bind:original_value="selected_character.dexterity" v-bind:dump_value="dump_d" v-bind:new_value="new_d" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="dex_pc" v-bind:minus_enabled="dex_mc"/>
      <DumpStat v-bind:stat_name="co" v-bind:original_value="selected_character.constitution" v-bind:dump_value="dump_co" v-bind:new_value="new_co" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="con_pc" v-bind:minus_enabled="con_mc"/>
      <DumpStat v-bind:stat_name="i" v-bind:original_value="selected_character.intelligence" v-bind:dump_value="dump_i" v-bind:new_value="new_i" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="int_pc" v-bind:minus_enabled="int_mc"/>
      <DumpStat v-bind:stat_name="w" v-bind:original_value="selected_character.wisdom" v-bind:dump_value="dump_w" v-bind:new_value="new_w" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="wis_pc" v-bind:minus_enabled="wis_mc"/>
      <DumpStat v-bind:stat_name="ch" v-bind:original_value="selected_character.charisma" v-bind:dump_value="dump_ch" v-bind:new_value="new_ch" @Plus="Plus" @Minus="Minus"
            v-bind:plus_enabled="cha_pc" v-bind:minus_enabled="cha_mc"/>
      </table>
      <h2 v-if="selected_character && selected_character.finalized==false && selected_character.username==username"
            @click="Finalize">
        Finalize {{ selected_character.name }} as a {{ character_role }} </h2>
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
        str_pc: false,
        str_mc: false,
        dex_pc: false,
        dex_mc: false,
        con_pc: false,
        con_mc: false,
        int_pc: false,
        int_mc: false,
        wis_pc: false,
        wis_mc: false,
        cha_pc: false,
        cha_mc: false,
        character_role: 'Fighter',
      errors: []
    }
  },

  props: {
    selected_character : Object,
    show_finalize : String,
    username: String
  },

  computed: {
    DumpTotal() {
        return this.dump_s + this.dump_d + this.dump_co + this.dump_i + this.dump_w + this.dump_ch;
    }
  },

  methods: {
    Plus(stat_name) {
        switch (stat_name) {
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
        this.UpdateFinalizeCharacterState();
    },
    Minus(stat_name) {
        switch (stat_name) {
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
        this.UpdateFinalizeCharacterState();
    },
    NewCoreValue() {
        switch (Number(this.c_role)) {
            case 0:
                this.new_s = this.selected_character.strength + this.DumpTotal - this.dump_s;
                this.character_role = 'Fighter';
                this.dump_s = 0;
                break;
            case 1:
                this.new_i = this.selected_character.intelligence +  this.DumpTotal - this.dump_i;
                this.character_role = 'Mage';
                this.dump_i = 0;
                break;
            case 2:
                this.new_d = this.selected_character.dexterity + this.DumpTotal - this.dump_d;
                this.character_role = 'Thief';
                this.dump_d = 0;
                break;
            case 3:
                this.new_w = this.selected_character.wisdom + this.DumpTotal - this.dump_w;
                this.character_role = 'Cleric';
                this.dump_w = 0;
                break;
        }
    },
    async ResetNewValues() {
        this.new_s = this.selected_character.strength;
        this.new_d = this.selected_character.dexterity;
        this.new_co = this.selected_character.constitution;
        this.new_i = this.selected_character.intelligence;
        this.new_w = this.selected_character.wisdom;
        this.new_ch = this.selected_character.charisma;
        this.dump_s = 0;
        this.dump_d = 0;
        this.dump_co = 0;
        this.dump_i = 0;
        this.dump_w = 0;
        this.dump_ch = 0;
        await this.DisableAllButtons();
        await this.UpdateFinalizeCharacterState();
    },
    async Finalize() {
        await HTTP.put('characters/'+this.selected_character.id+'/',
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
    },
    DumpPlusRules(DumpValue) {
        if (DumpValue <= 9) {
            return false;
        }
        return true;
    },
    DumpMinusRules(DumpValue) {
        if (DumpValue <= 0) {
            return false;
        }
        return true;
    },
    EnforceFinalizationRules() {
        this.str_pc = this.DumpPlusRules( this.new_s );
        this.dex_pc = this.DumpPlusRules( this.new_d );
        this.con_pc = this.DumpPlusRules( this.new_co );
        this.int_pc = this.DumpPlusRules( this.new_i );
        this.wis_pc = this.DumpPlusRules( this.new_w );
        this.cha_pc = this.DumpPlusRules( this.new_ch );

        this.str_mc = this.DumpMinusRules( this.dump_s );
        this.dex_mc = this.DumpMinusRules( this.dump_d );
        this.con_mc = this.DumpMinusRules( this.dump_co );
        this.int_mc = this.DumpMinusRules( this.dump_i );
        this.wis_mc = this.DumpMinusRules( this.dump_w );
        this.cha_mc = this.DumpMinusRules( this.dump_ch );

        switch (Number(this.c_role)) {
            case 0:
                this.str_pc = false;
                this.str_mc = false;
                if (this.new_s >= 18)
                    { this.DisableAllPlusButtons() }
                break;
            case 1:
                this.int_pc = false;
                this.int_mc = false;
                if (this.new_i >= 18)
                    { this.DisableAllPlusButtons() }
                break;
            case 2:
                this.dex_pc = false;
                this.dex_mc = false;
                if (this.new_d >= 18)
                    { this.DisableAllPlusButtons() }
                break;
            case 3:
                this.wis_pc = false;
                this.wis_mc = false;
                if (this.new_w >= 18)
                    { this.DisableAllPlusButtons() }
                break;
        }
        console.log('AFTER ENFORCE RULES');
        console.log(this.c_role + ' ' + this.character_role);
        console.log(this.str_pc + ' ' + this.str_mc);
        console.log(this.dex_pc + ' ' + this.dex_mc);
        console.log(this.con_pc + ' ' + this.con_mc);
        console.log(this.int_pc + ' ' + this.int_mc);
        console.log(this.wis_pc + ' ' + this.wis_mc);
        console.log(this.cha_pc + ' ' + this.cha_mc);
    },
    EnableAllButtons() {
        this.str_pc = true;
        this.str_mc = true;
        this.dex_pc = true;
        this.dex_mc = true;
        this.con_pc = true;
        this.con_mc = true;
        this.int_pc = true;
        this.int_mc = true;
        this.wis_pc = true;
        this.wis_mc = true;
        this.cha_pc = true;
        this.cha_mc = true;
    },
    DisableAllButtons() {
        this.str_pc = false;
        this.str_mc = false;
        this.dex_pc = false;
        this.dex_mc = false;
        this.con_pc = false;
        this.con_mc = false;
        this.int_pc = false;
        this.int_mc = false;
        this.wis_pc = false;
        this.wis_mc = false;
        this.cha_pc = false;
        this.cha_mc = false;
    },
    DisableAllPlusButtons() {
        this.str_pc = false;
        this.dex_pc = false;
        this.con_pc = false;
        this.int_pc = false;
        this.wis_pc = false;
        this.cha_pc = false;
    },
    UpdateFinalizeCharacterState() {
        console.log('BEFORE VALUES');
        console.log(this.c_role + ' ' + this.character_role);
        console.log(this.str_pc + ' ' + this.str_mc);
        console.log(this.dex_pc + ' ' + this.dex_mc);
        console.log(this.con_pc + ' ' + this.con_mc);
        console.log(this.int_pc + ' ' + this.int_mc);
        console.log(this.wis_pc + ' ' + this.wis_mc);
        console.log(this.cha_pc + ' ' + this.cha_mc);
        this.NewCoreValue();
        this.EnforceFinalizationRules();
    }
  },
  watch: {
        selected_character() {
            this.c_role = 0;
            this.character_role = 'Fighter';
            if (this.selected_character.intelligence > this.selected_character.strength
             && this.selected_character.intelligence > this.selected_character.dexterity
             && this.selected_character.intelligence > this.selected_character.wisdom) {
                this.c_role = 1;
                this.character_role = 'Mage';
            }
            if (this.selected_character.dexterity > this.selected_character.strength
             && this.selected_character.dexterity > this.selected_character.intelligence
             && this.selected_character.dexterity > this.selected_character.wisdom) {
                this.c_role = 2;
                this.character_role = 'Thief';
            }
            if (this.selected_character.wisdom > this.selected_character.strength
             && this.selected_character.wisdom > this.selected_character.dexterity
             && this.selected_character.wisdom > this.selected_character.intelligence) {
                this.c_role = 3;
                this.character_role = 'Cleric';
            }
            this.ResetNewValues();
        },
        show_finalize() {
            this.ResetNewValues();
        }
  }
}
</script>