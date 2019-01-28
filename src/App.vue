<template>
  <v-app>
    <v-toolbar color="primary" fixed  app>
      <v-toolbar-side-icon></v-toolbar-side-icon>
      <v-toolbar-title>Bank Search</v-toolbar-title>
    </v-toolbar>

    <v-content>
      <search />
    </v-content>
  </v-app>
</template>

<script>
import Search from './components/Search'

export default {
  name: 'App',
  components: {
    'search': Search
  },
  data () {
    return {
      //
    }
  }
}
</script>
