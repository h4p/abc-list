<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar fixed app dark prominent src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-menu
      open-on-hover
      offset-y
    >
      <template #activator="{ on, attrs }">
        <v-btn
          class="ma-2"
          medium
          outlined
          fab
          v-bind="attrs"
          v-on="on"
        >
          <v-icon>mdi-account</v-icon>
        </v-btn>
      </template>

      <v-list>
        <v-list-item
        v-if="$auth.loggedIn == false"
        to="/login"
        >
        <v-list-item-title>Login</v-list-item-title>
        </v-list-item>

        <v-list-item
        v-if="$auth.loggedIn == true"
        @click="logoutUser()"
        >
        <v-list-item-title>Logout</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
        dark
        padless
      >
        <v-card
          flat
          tile
          class="text-center"
          width="100%"
        >
          <v-card-text class="white--text">
            (c){{ new Date().getFullYear() }} — <strong><a href="https://github.com/h4p" target="new">h4p</a></strong>
          </v-card-text>
        </v-card>
      </v-footer>

      <v-snackbar v-model="snackbar" :timeout="timeout" :multi-line="multiLine" top>
        {{ snackbarMessage }}

        <template #action="{ attrs }">
          <v-btn
            color="pink"
            text
            v-bind="attrs"
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>


  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-library',
          title: 'Overview',
          to: '/overview',
        },
        {
          icon: 'mdi-check-decagram',
          title: 'About',
          to: '/about',
        },
        {
          icon: 'mdi-file-plus',
          title: 'New',
          to: '/list',
        },
        // {
        //   icon: 'mdi-login',
        //   title: 'Login',
        //   to: '/login',
        // },
      ],
      miniVariant: false,
      right: true,
      title: 'ABC Liste',
      profile_menu_items: [
        { 'title': 'My settings' },
        { 'title': 'Logout' }
      ],

      // Snackbar
      snackbar: false,
      timeout: 2000,
      multiLine: true,
      snackbarMessage: 'Hello from snackbar!',

    }
  },
  created() {
    this.$root.$on('showSnackbar', message => {
      this.snackbar = true
      this.snackbarMessage = message;
    })
  },
  beforeDestroy(){
    this.$root.$off('showSnackbar')
  },
  methods: {
    async logoutUser() {
      this.$auth.$storage.removeUniversal('user')

      await this.$auth.logout({
          data: {
              refreshToken: this.$auth.strategy.refreshToken.get().replaceAll('Bearer ', '').replaceAll('"', '')
          }
      });

      this.$router.push('/')
      this.snackbar = true
      this.text = "Logout successful"
    },
  }
}
</script>
