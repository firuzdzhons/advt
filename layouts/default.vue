<template>
  <v-app>
    <v-app-bar
      app 
    >
      <v-toolbar-title>
        <nuxt-link to="/">ADVT</nuxt-link>
      </v-toolbar-title>
      <v-spacer />
      <v-btn  v-if="$nuxt.$route.path != '/advertisements/create'" to="/advertisements/create"
          depressed
          color="primary"
        >
          Создать
    </v-btn>

    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      title: 'ADVT'
    }
  }
}
</script>
