<template>
  <span>
    <v-row>
      <v-col>
        <h3>DISTANCE</h3>
      </v-col>
    </v-row>
    <v-row justify="space-between" align="center">
      <v-col>
        <v-text-field
          :value="value.distance"
          outlined
          @input="handleInput('distance', $event)"
        />
      </v-col>
      <v-col>
        <choices
          class="p-0 m-2"
          :value="value.unit"
          :items="['yards', 'meters']"
          @input="handleInput('unit', $event)"
        />
      </v-col>
    </v-row>
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

<style scoped>
.p-0 {
  padding: 0;
}
.m-2 {
  margin: -2rem 0 0 0;
}
</style>
