<template>
  <v-card raised>
    <v-card-title class="primary font-weight-bold">
      Processes
    </v-card-title>
    <v-card-text
      v-if="app.State.Status != 'running'"
      class="secondary text-center px-5 py-5"
    >
      Start the app to view processes
    </v-card-text>
    <v-simple-table v-else-if="processes">
      <template v-slot:default>
        <thead>
          <tr>
            <th
              class="secondary"
              v-for="(title, index) in processes.Titles"
              :key="index"
            >
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody class="secondary">
          <tr v-for="(item, index) in processes.Processes" :key="index">
            <td v-for="(field, index) in item" :key="index">{{ field }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-card-text v-else>
      Something Broke >:(
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["app", "processes"],
  search: ""
};
</script>
