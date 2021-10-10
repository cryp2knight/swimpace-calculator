<template>
  <span>
    <v-row>
      <v-col>
        <h1>{{ type }}</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="(value, key) in result" :key="key">
        <v-text-field :value="value" :label="key" outlined readonly />
      </v-col>
    </v-row>
  </span>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      required: true
    },
    result: {
      type: Object,
      default: () => {}
    }
  }
}
</script>
