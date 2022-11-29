<template>
  <v-row>
    <v-col class="text-center">
      <v-simple-table>
          <thead>
            <tr>
              <th class="text-center"></th>
              <th class="text-center">Topic</th>
              <th class="text-center">Last modified by me</th>
              <th class="text-center">Completed</th>
              <th class="text-center">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="item in lists.results" :key="item.text">
              <td><v-icon>mdi-book</v-icon></td>
              <td>{{ item.topic }}</td>
              <td>{{ formatDate(item.updatedAt) }}</td>
              <td>{{ countListItems(item.abclist) }} / 26</td>
              <td><v-btn color="primary" nuxt :to="{path: '/list',query:{abclistId:item._id}}">Open</v-btn></td>
            </tr>
          </tbody>
      </v-simple-table>


    </v-col>
  </v-row>
</template>

<script>
export default {
    name: 'OverviewPage',
    middleware: 'auth',

    data: () => ({
      selectedItem: 1,
      lists: [
        { id: 1, text: '2022-01-02 14:02:01', completed: 12, thema: 'Städte' },
        { id: 2, text: '2022-01-04 11:12:01', completed: 5, thema: 'Tiere' },
        { id: 3, text: '2022-01-06 17:22:01', completed: 26, thema: 'Raketen' },
        { id: 4, text: '2022-01-07 17:26:01', completed: 26, thema: 'Coding' },
      ],
    }),

    // **** Lifecycle Hooks **** //
    created() {
      // eslint-disable-next-line no-console
      console.log('Component has been created!');
      // The advantage of using created instead of mounted is that
      // created will be called a little sooner. This means you'll
      // get your data just a tiny bit faster.
      // Created hooks doesn't have access to the DOM.

      // eslint-disable-next-line no-console
      console.log("Fetching abclists")
      this.$axios.$get('http://localhost:3000/v1/abclists')
        .then((lists) => {
          this.lists = lists;
        })
        .catch((err) => {
          // If any part of the chain is rejected, print the error message.
          console.log(err);
        });

    },
    mounted() {
      // eslint-disable-next-line no-console
      console.log('Component has been mounted!');

      // DOM is ready by now. Component was added to DOM
    },
    destroyed() {
      // eslint-disable-next-line no-console
      console.log('Component has been destroyed!');
    },
    methods: {
      formatDate(date) {
        const options = { year: 'numeric', month: 'numeric', day: 'numeric', hour: 'numeric', minute: 'numeric' }
        return new Date(date).toLocaleDateString('de', options)
      },
      countListItems(liste) {
        let count = 0;
        Object.keys(liste).forEach((key) => {
          if(liste[key] !== null && liste[key] !== '') {
            count++;
          }
        });
        return count;
      },
    },
  }

</script>
