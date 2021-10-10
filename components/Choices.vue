<template>
  <v-row justify="space-between">
    <v-col v-for="(item, i) in items" :key="i">
      <v-btn
        x-large
        depressed
        block
        :color="value === item ? `${main ? 'error' : 'primary'}` : 'normal'"
        :height="main ? 100 : undefined"
        :style="main && 'font-size: 1.5rem; font-weight: 600'"
        @click="$emit('input', item)"
      >
        {{ item }}
      </v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      default: () => []
    },
    value: {
      type: [String, Number, Object],
      default: () => null
    },
    main: {
      type: Boolean
    }
  }
}
</script>
