<template>
  <v-form>
    <v-container>

      <v-row>
        <v-col>
          <v-text-field :id="topic" v-model="topic" label="Thema" filled outlined prepend-inner-icon="mdi-order-alphabetical-ascending"></v-text-field>
        </v-col>
      </v-row>

      <v-row v-if="abc_list">
          <v-col v-for="letter in Object.keys(abc_list)" :key="letter" cols="12" sm="6" >
            <v-text-field  :id="letter" v-model="abc_list[letter]" :label="letter.toUpperCase()" single-line filled />
          </v-col>
      </v-row>

      <v-row v-else>
        <p>Call to action: Create new list</p>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: 'ListPage',
  topic: '',
  data: () => ({
    abc_list: {'a': 'Ameland','b': 'Beland','c': '','d': '','e': '','f': '','g': '','h': '','i': '','j': '','k': '','l': '','m': '','n': '','o': '','p': '','q': '','r': '','s': '','t': '','u': '','v': '','w': '','x': '','y': '','z': ''},
  }),
}
</script>
