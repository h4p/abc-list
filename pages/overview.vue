<template>
  <v-row>
    <v-col class="text-center">
      <v-simple-table>
          <thead>
            <tr>
              <th class="text-center">Icon</th>
              <th class="text-center">Thema</th>
              <th class="text-center">Creation date</th>
              <th class="text-center">Completed</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="item in lists" :key="item.text">
              <td><v-icon>mdi-book</v-icon></td>
              <td>{{ item.thema }}</td>
              <td><NuxtLink to="/about/$route.params.">{{ item.text }}</NuxtLink></td>
              <td>{{ item.completed }}</td>
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
    })
  }

</script>
