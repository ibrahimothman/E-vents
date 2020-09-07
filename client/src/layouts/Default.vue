<template>
  <v-app>
    <!-- app bar -->
    <v-app-bar app>
      <!-- title -->
      <v-toolbar-title>
        <g-link to="/">
        {{ $static.metadata.siteName }}</g-link></v-toolbar-title>
      <!-- search text field -->
      <v-text-field
        v-model="searchField"
        @click:clear="searchField = ''"
        placeholder="Search"
        class="ml-8"
        style="max-width:300px;"
        prepend-inner-icon="mdi-magnify"
        outlined
        rounded
        clearable
        dense
        hide-details
      ></v-text-field>
      <v-spacer/>
      <!-- create a new event -->
      <v-btn>Create Event</v-btn>
    </v-app-bar>

     <!-- routes -->
    <v-main>
      <v-container>
        <v-row>
          <v-col sm="10" offset-sm="1">
            <slot :searchField="searchField"/>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<static-query>
query {
  metadata {
    siteName,
    siteDescription,
  }
}
</static-query>

<script>
export default {
  data: () => ({
    searchField: '',
  }),
}
</script>
<style>
  .v-toolbar__title a {
    text-decoration: none;
  }
</style>
