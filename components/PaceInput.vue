<template>
  <span>
    <v-row><v-col><h3>PACE</h3></v-col></v-row>
    <v-row justify="space-between">
      <v-col>
        <v-text-field
          :value="value.minutes"
          label="Minutes"
          outlined
          @input="handleInput('minutes', $event)"
        />
      </v-col>
      <v-col>
        <v-text-field
          :value="value.seconds"
          label="Seconds"
          outlined
          @input="handleInput('seconds', $event)"
        />
      </v-col>
      <v-col>
        <v-text-field
          :value="value.ms"
          label="MS"
          outlined
          @input="handleInput('ms', $event)"
        />
      </v-col>
    </v-row>
    <choices
      :value="value.choice"
      :items="items"
      @input="handleInput('choice', $event)"
    />
  </span>
</template>

<script>
import Choices from './Choices.vue'
export default {
  components: { Choices },
  props: {
    value: {
      type: Object,
      default: () => {}
    }
  },
  data () {
    const items = ['min/100yds', 'min/100m']
    return {
      items,
      input: {
        ...this.value
      }
    }
  },
  methods: {
    handleInput (key, val) {
      this.input[key] = val
      this.$emit('input', this.input)
    }
  }
}
</script>
