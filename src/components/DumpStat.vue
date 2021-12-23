<template>
      <tr><td>{{ stat_name }}</td>
      <td class=CharacterStatistic><b>{{ original_value }}</b></td>
      <td class=DumpValue>Dump {{ dump_value }}</td>
      <td class=DumpValue :id="stat_name" @click="Plus"><b>+</b></td>
      <td class=DumpValue :id="stat_name" @click="Minus"><b>-</b></td>
      <td>Final Value {{ new_value }}</td>
      </tr>
</template>

<script>

export default {
  data() {
    return {
      errors: []
    }
  },

  props: {
    stat_name : String,
    original_value : Number,
    dump_value: Number,
    new_value: Number
  },

  created() {
  },

  methods: {
        Plus() {
            this.$emit('Plus', this.stat_name);
        },
        Minus() {
            this.$emit('Minus', this.stat_name);
        }
  }
}
</script>