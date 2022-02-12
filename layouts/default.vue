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
      <!-- <v-btn
        class="ma-2"
        small
        outlined
        fab
      >
      <v-icon>mdi-account</v-icon>
      </v-btn> -->
      <v-menu
      open-on-hover
      top
      offset-y
    >
      <template v-slot:activator="{ on, attrs }">
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
          v-for="(item, index) in profile_menu_items"
          :key="index"
        >
          <v-list-item-title><a href="/foo">{{ item.title }}</a></v-list-item-title>
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
      ],
      miniVariant: false,
      right: true,
      title: 'ABC Liste',
      profile_menu_items: [
        { 'title': 'My settings' },
        { 'title': 'Logout' }
      ],
    }
  },
}
</script>
