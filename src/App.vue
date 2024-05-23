<template>
  <div id="app">
    <h1 class="title">Rick and Morty Characters</h1>
    <div class="filters">
      <input v-model="tempNameFilter" placeholder="Filter by name" />
      <select v-model="tempStatusFilter">
        <option value="">All Statuses</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Apply</button>
    </div>
    <div class="character-grid">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CharacterCard from './components/CharacterCard.vue';

export default {
  name: 'App',
  components: {
    CharacterCard
  },
  data() {
    return {
      characters: [],
      currentPage: 1,
      totalPages: 1
    };
  },
  created() {
    this.fetchCharacters();
  },
  methods: {
    async fetchCharacters() {
      try {
        const response = await axios.get(`https://rickandmortyapi.com/api/character/`, {
          params: {
            page: this.currentPage,
            name: this.nameFilter,
            status: this.statusFilter
          }
        });
        this.characters = response.data.results;
        this.totalPages = response.data.info.pages;
      } catch (error) {
        console.error('Error fetching characters:', error);
      }
    },
    async applyFilters() {
      this.currentPage = 1; // Reset to first page when applying filters
      this.nameFilter = this.tempNameFilter;
      this.statusFilter = this.tempStatusFilter;
      await this.fetchCharacters();
    },
    async prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        await this.fetchCharacters();
      }
    },
    async nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        await this.fetchCharacters();
      }
    }
  }
}
</script>

<style>
#app {
  text-align: center;
}
.title {
  font-size: 5.625rem;
  font-weight: 900;
  font-family: -apple-system,'BlinkMacSystemFont','Segoe UI','Roboto','Helvetica','Arial',sans-serif,'Apple Color Emoji','Segoe UI Emoji','Segoe UI Symbol';
}

.character-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.filters {
  margin: 20px;
  display: flex;
  gap: 10px;
}

.filters input,
.filters select,
.filters button {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.character-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.pagination {
  margin-top: 20px;
  font-family: -apple-system,'BlinkMacSystemFont','Segoe UI','Roboto','Helvetica','Arial',sans-serif,'Apple Color Emoji','Segoe UI Emoji','Segoe UI Symbol';
  color: black;
}

.pagination button {
  margin: 0 10px;
  padding: 10px 20px;
  font-family: -apple-system,'BlinkMacSystemFont','Segoe UI','Roboto','Helvetica','Arial',sans-serif,'Apple Color Emoji','Segoe UI Emoji','Segoe UI Symbol';
  color: white;
  font-size: 1rem;
  color: white;
  background-color: rgb(60, 62, 68);

  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.pagination button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.pagination span {
  font-size: 1.2rem;
}
</style>
