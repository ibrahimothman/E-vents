<template>
  <Layout v-slot="{ searchField }">

    <v-tabs  v-model="tab" grow>
      <v-tab>All</v-tab>
      <v-tab>Music</v-tab>
      <v-tab>Code</v-tab>
    </v-tabs>

      <v-row class="justify-space-around">
        <v-card
          v-for="{ node:event } in getEvents(searchField)" :key="event.id"
          width="350"
          class="mt-5"
        >
          <v-img
            class="white--text align-end"
            height="200px"
            :src="`http://localhost:1337${event.thumbnail}`"
          />

          <v-card-title>{{ event.title }}</v-card-title>

          <v-card-subtitle class="pb-0">{{ formatDate(event.date) }}</v-card-subtitle>

          <v-card-actions>
            <v-btn
            @click="$router.push(`/events/${event.id}`)"
              color="orange"
              text
            >
              More Info
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-row>


    <!-- <pre>{{ $page.allEvents.edges }}</pre> -->

  </Layout>
</template>

<page-query>
  query {
    allEvents: allEvent {
      edges {
        node {
          id,
          title,
          description,
          thumbnail,
          price,
          duration,
          date,
          categories{
            id,
            name,
          },
        }
      }
    }
}
</page-query>
<script>
import moment from 'moment';
export default {
  metaInfo: {
    title: 'Hello, world!'
  },
  data: () => ({
    tab: 0,
    events: [],
  }),
  mounted(){
    this.events = this.$page.allEvents.edges;
  },
  watch: {
    tab(val) {
      if(val === 0) {
        this.showAllEvents();
      }else {
        this.showEventsByType(val);
      }
    }
  },
  methods: {
    showAllEvents() {
      this.events = this.$page.allEvents.edges;
    },
    showEventsByType(val) {
      this.events = this.$page.allEvents.edges.filter(({ node:event }) => {
        return event.categories.find(category => category.id === val + 3);
      })
    },
    formatDate(date) {
      return moment(date).format('MMMM Do YYYY, h:mm:ss a');
    },
    getEvents(searchField) {
      if (searchField){
        console.log('search....', searchField);
        return this.events.filter(({ node:event }) => {
          return event.title.toLowerCase().includes(searchField.toLowerCase());
        })
      }else {
        return this.events;
      }
    }
  },
}
</script>
