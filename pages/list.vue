<template>
  <v-form @submit.prevent="updateList">
    <v-container>

      <v-row>
        <v-col class="text-right">
          <v-btn color="primary" type="submit">Save</v-btn>
          <v-btn color="tertiary" type="submit">Abort</v-btn>
        </v-col>
      </v-row>

      <v-row v-if="abcList">
        <v-col>
          <v-text-field v-model="abcList.topic" label="Thema" single-line filled outlined prepend-icon="mdi-format-list-checkbox"></v-text-field>
        </v-col>
      </v-row>

      <v-row v-if="abcList">
          <v-col v-for="(value, letter) in abcList.abclist" :key="letter" cols="12" sm="6" >
            <v-text-field  :id="letter" v-model="abcList.abclist[letter]" :label="letter.toUpperCase()" single-line filled />
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
  data: () => {
    return ({
      abcList: {
        "_id": "6218012bf6683b15d099611f",
        "topic": "Food",
        "abclist": { 'a': '', 'b': '', 'c': '', 'd': '', 'e': '', 'f': '', 'g': '', 'h': '', 'i': '', 'j': '', 'k': '', 'l': '', 'm': '', 'n': '', 'o': '', 'p': '', 'q': '', 'r': '', 's': '', 't': '', 'u': '', 'v': '', 'w': '', 'x': '', 'y': '', 'z': '' },
        "user": "6210150cd1da4800c2050493",
        "createdAt": "2022-02-24T22:05:31.610Z",
        "updatedAt": "2022-02-25T20:36:12.686Z",
        "id": "6218012bf6683b15d099611f"
      }
    });
  },
    // **** Lifecycle Hooks **** //
    created() {
      console.log('Component has been created!');
      // The advantage of using created instead of mounted is that
      // created will be called a little sooner. This means you'll
      // get your data just a tiny bit faster.
      // Created hooks doesn't have access to the DOM.

      // Log query params
      // console.log(this.$route.query)
      // console.log(this.$route.query.plan)
      // console.log(this.$nuxt._route.query)
      // console.log(this.$nuxt._route.query.plan)


      console.log("Fetching abclists")
      this.$axios.$get('http://localhost:3000/v1/abclists/'+this.$route.query.abclistId)
        .then((abcList) => {
          const emptyAbcList = this.abcList.abclist;
          for(const [key, value] of Object.entries(abcList.abclist)) {
            emptyAbcList[key] = value;
          }
          this.abcList = abcList; // overwrite
          this.abcList.abclist = emptyAbcList;
          console.log("Got abclist: %O", this.abcList);
        })
        .catch((err) => {
          // If any part of the chain is rejected, print the error message.
          console.log(err);
        });


    },
    mounted() {
      console.log('Component has been mounted!');

      // DOM is ready by now. Component was added to DOM
    },
    destroyed() {
      console.log('Component has been destroyed!');
    },
    methods: {
      async updateList() {
        await this.$axios.$put('http://localhost:3000/v1/abclists/'+this.$route.query.abclistId, this.abcList)
          .then((lists) => {
            this.lists = lists;
          })
          .catch((err) => {
            // If any part of the chain is rejected, print the error message.
            console.log(err);
          });
      }
    },
}
</script>
