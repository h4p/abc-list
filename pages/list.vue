<template>
  <v-form @submit.prevent="updateList">
    <v-container>

      <v-row v-if="abcList">
        <v-col>
          <v-text-field v-model="abcList.topic" label="Thema" single-line filled outlined prepend-icon="mdi-format-list-checkbox"></v-text-field>
        </v-col>
      </v-row>

      <v-row v-if="abcList">
          <v-col v-for="(value, letter) in abcList.abclist" :key="letter" cols="12" sm="6" class="letters">
            <v-text-field  :id="letter" v-model="abcList.abclist[letter]" :label="letter.toUpperCase()" single-line filled outlined dense />
          </v-col>
      </v-row>

      <v-row>
        <v-col class="text-right">
          <v-btn color="primary" type="submit">{{ isUpdate ? 'Update' : 'Save'}}</v-btn>
          <v-btn color="tertiary" nuxt :to="{path: '/overview'}">Abort</v-btn>
        </v-col>
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
        "_id": "",
        "topic": "",
        "abclist": { 'a': '', 'b': '', 'c': '', 'd': '', 'e': '', 'f': '', 'g': '', 'h': '', 'i': '', 'j': '', 'k': '', 'l': '', 'm': '', 'n': '', 'o': '', 'p': '', 'q': '', 'r': '', 's': '', 't': '', 'u': '', 'v': '', 'w': '', 'x': '', 'y': '', 'z': '' },
        "user": "",
        "createdAt": "",
        "updatedAt": "",
        "id": ""
      },
      isUpdate: false,
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

      if(this.$route.query.abclistId === null || this.$route.query.abclistId === undefined) {
        this.$router.app.refresh();
        console.log("Request with no query params. User wants to create a new list");
        this.isUpdate = false;
        return;
      }

      console.log("Fetching abclists")
      this.isUpdate = true;
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

        if (this.isUpdate) {
          console.log('Patching..');

          // Prepare json object for update
          const updatedAbclist = Object.assign(this.abcList);
          delete updatedAbclist._id;
          delete updatedAbclist.id;
          delete updatedAbclist.user;
          delete updatedAbclist.createdAt;
          delete updatedAbclist.updatedAt;

          await this.$axios.$patch('http://localhost:3000/v1/abclists/'+this.$route.query.abclistId, updatedAbclist)
            .then((lists) => {
              console.log(lists);
              this.lists = lists;
              this.$root.$emit('showSnackbar', 'List was updated');

            })
            .catch((err) => {
              // If any part of the chain is rejected, print the error message.
              this.$root.$emit('showSnackbar', 'Oops! An error occurred!');
              console.log(err);
            });

        }
        else {
          console.log("Posting.");
          // Prepare json object for insert
          const updatedAbclist = Object.assign(this.abcList);
          delete updatedAbclist._id;
          delete updatedAbclist.id;
          delete updatedAbclist.user;
          delete updatedAbclist.createdAt;
          delete updatedAbclist.updatedAt;

          await this.$axios.$post('http://localhost:3000/v1/abclists/', updatedAbclist)
            .then((lists) => {
              console.log(lists);
              this.lists = lists;
              this.$root.$emit('showSnackbar', 'List was created');

            })
            .catch((err) => {
              // If any part of the chain is rejected, print the error message.
              this.$root.$emit('showSnackbar', 'Oops! An error occurred!');
              console.log(err);
            });

        }
      }
    },
}
</script>
<style scoped>
.letters {
    padding-top: 0;
    padding-bottom: 0;
}
</style>
