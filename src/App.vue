<template>
  <div class="container">
    <h1>{{ title }}</h1>
    <div class="button-container">
      <button @click="addRandomContact">Add Random Contact</button>
      <button @click="sortByPopularity">Sort By Popularity</button>
      <button @click="sortByName">Sort By Name</button>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won an Oscar</th>
          <th>Won an Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" alt="contact picture"></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td v-if="contact.wonOscar">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy">🏆</td>
          <td v-else></td>
          <td><button @click="deleteContact(contact.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import contacts from './contacts.json';
export default {
  name: 'App',
  data() {
    return {
      title: 'IronContacts',
      contacts: [],
      remainingContacts: [],
      sortOrder: 'desc',
    };
  },
  mounted() {
    this.contacts = contacts.slice(0, 5);
    this.remainingContacts = contacts.slice(5);
  },
  methods: {
    addRandomContact() {
      if (this.remainingContacts.length === 0) {
        alert('No more contacts available.');
        return;
      }
      const randomIndex = Math.floor(Math.random() * this.remainingContacts.length);
      const newContact = this.remainingContacts.splice(randomIndex, 1)[0];
      this.contacts.push(newContact);
    },
    sortByPopularity() {
      if (this.sortOrder === 'asc') {
        this.contacts.sort((a, b) => a.popularity - b.popularity);
        this.sortOrder = 'desc';
      } else {
        this.contacts.sort((a, b) => b.popularity - a.popularity);
        this.sortOrder = 'asc';
      }
    },
    sortByName() {
      if (this.sortOrder === 'asc') {
        this.contacts.sort((a, b) => a.name.localeCompare(b.name));
        this.sortOrder = 'desc';
      } else {
        this.contacts.sort((a, b) => b.name.localeCompare(a.name));
        this.sortOrder = 'asc';
      }
    },
    deleteContact(id) {
      const index = this.contacts.findIndex(contact => contact.id === id);
      if (index !== -1) {
        this.contacts.splice(index, 1);
      }
    }
  }
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  font-weight: bold;
  text-align: center;
  margin-bottom: 20px;
}

.button-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.button-container button {
  margin-right: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.table {
  width: 100%;
  border-collapse: collapse;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  /* Adding shadow */
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

td img {
  max-width: 100px;
  height: auto;
}
</style>
