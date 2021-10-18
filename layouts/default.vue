<template>
  <v-app>
    <v-app-bar
      v-if="hasappbar"
      clipped-left
      fixed
      app
    >
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-container v-else>
      <h1 class="cyan--text text--darken-2">
        {{ title }}
      </h1>
      <v-divider />
    </v-container>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      title: 'Swim pace calculator'
    }
  },
  computed: {
    hasappbar () {
      return !!this.$route?.query?.appbar
    }
  }
}
</script>

<style>
.row {
  margin: 0;
}
</style>
