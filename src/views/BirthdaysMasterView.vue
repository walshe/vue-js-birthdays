<script>
const API_URL = `https://randomuser.me/api/?results=10&seed=chalkboard&inc=id,name,dob`;

export default {
  data: () => ({
    birthdays: null,
    page: 1,
  }),

  created() {
    
    // fetch on init
    this.fetchData();
  },

  watch: {
    // re-fetch whenever page value changes
    page: "fetchData",
  },

  methods: {
    previous() {
      if (this.page != 1) {
        this.page--;
      }
    },

    next() {
      this.page++;
    },

    async fetchData() {
      const url = `${API_URL}&page=${this.page}`;
      const json = await (await fetch(url)).json();
      this.birthdays = json.results;
    },
    truncate(v) {
      const newline = v.indexOf("\n");
      return newline > 0 ? v.slice(0, newline) : v;
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return `${date.getDate()}/${date.getMonth()}/${date.getFullYear()}`;
    },
  },
};
</script>

<template>
  <main>
    Birthdays.. 
    <ul>
      <li v-for="{ name, dob } in birthdays">
        
        <router-link
        :to="{
          name: 'birthday',
          params: {
            name: `${name.title} ${name.first} ${name.last}`,
            age: dob.age
          }
        }">
        {{ name.title }} {{ name.first }} {{ name.last }}&nbsp;{{ formatDate(dob.date) }}
      </router-link>
      </li>

      <button v-bind:disabled="page == 1" @click="previous()">Previous page</button>
      <button @click="next()">Next page</button>
    </ul>
  </main>
</template>
