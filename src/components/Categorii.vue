<script>
import axios from 'axios';
export default {
  data() {
    return {
      names: [],          // Holds the names fetched from the API or database
      selectedName: null, // Holds the currently selected name
    };
  },
  mounted() {
  axios.get('http://localhost:8000/api/categorii').then(response => {
    this.names = response.data;
    }).catch(error => {
        console.log(error);
        });
        },
  methods: {
    showCategory(name) {
      this.selectedName = name;
    },
  },
};
</script>



<template>
  <div>
    <ul>
      <li v-for="name in names" :key="name.id" @click="showCategory(name)">
        {{ name.denumire }}
      </li>
    </ul>
    <div v-if="selectedName">
      <h2>{{ selectedName.denumire }}</h2>
      <p>Category: {{ selectedName.subcategorie }}</p>
    </div>
  </div>
</template>




<style scoped>

</style>