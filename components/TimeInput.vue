<template>
  <span>
    <v-row><v-col><h3>TIME</h3></v-col></v-row>
    <v-row justify="space-between">
      <v-col>
        <v-text-field
          :value="value.hours"
          label="Hours"
          outlined
          @input="handleInput('hours', $event)"
        />
      </v-col>
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
    </v-row>
  </span>
</template>

<script>
export default {
  props: {
    value: {
      type: Object,
      default: () => {}
    }
  },
  data () {
    return {
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
