<template>
  <v-app dark>
    <v-app-bar
      clipped-left
      fixed
      app
    >
      <v-toolbar-title v-text="title" />
    </v-app-bar>
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
  }
}
</script>

<style>
.row {
  margin: 0;
}
</style>
