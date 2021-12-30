<template>
      <tr><td>{{ stat_name }}</td>
      <td class=CharacterStatistic><b>{{ new_value }}</b></td>
      <td class=DumpValue>Dump {{ dump_value }}</td>
      <td :class="plus_class" :id="stat_name" @click="Plus"><b>+</b></td>
      <td :class="minus_class" :id="stat_name" @click="Minus"><b>-</b></td>
      </tr>
</template>

<script>

export default {
  data() {
    return {
        plus_class: 'DumpControlDisabled',
        minus_class: 'DumpControlDisabled',
      errors: []
    }
  },

  props: {
    stat_name : String,
    original_value : Number,
    dump_value: Number,
    new_value: Number,
    plus_enabled: Boolean,
    minus_enabled: Boolean
  },

  methods: {
        Plus() {
            if (this.plus_enabled) {
                this.$emit('Plus', this.stat_name);
            }
        },
        Minus() {
            if (this.minus_enabled) {
                this.$emit('Minus', this.stat_name);
            }
        }
  },

  watch: {
        plus_enabled(val) {
            if (val==false) {
                this.plus_class = 'DumpControlDisabled'
            }
            else {
                this.plus_class = 'DumpControlEnabled'
            }
        },

        minus_enabled(val) {
            if (val==false) {
                this.minus_class = 'DumpControlDisabled'
            }
            else {
                this.minus_class = 'DumpControlEnabled'
            }
        }
  }
}
</script>