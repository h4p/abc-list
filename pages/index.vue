<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          Willkommen auf ABC-Liste.de
        </v-card-title>
        <v-card-text>
          <p>
            ABC-Listen sind ein gutes Mittel, um Informationen abrufbereit zu halten. Sie funktionieren ähnlich wie das Spiel <em>Stadt-Land-Fluss</em>, aber hier bestimmen Sie das Thema selbst!
          </p>

          <p>
            Menschliche Gehirne verstärken Nervenbahnen, wenn diese oft genutzt werden. Aus einem kleinen Trampelpfad kann so eine Straße und sogar eine Autobahn entstehen.
          </p>

        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" nuxt to="/list">Liste erstellen</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
}
</script>
