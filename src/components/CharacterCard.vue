<template>
  <div class="character-card">
    <img :src="character.image" :alt="character.name" />
    <div class="character-info">
      <div class="section">
        <h2>{{ character.name }}</h2>
        <p><span :class="statusClass">{{ character.status }}</span> - {{ character.species }}</p>
      </div>
      <div class="section">
        <span class="gray-label">Last known location:</span> 
        <p>{{ character.location.name }}</p>
      </div>
      <div class="section">
          <span class="gray-label">First seen in:</span> 
          <p>{{ firstEpisodeName }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    character: Object
  },
  data() {
    return {
      firstEpisodeName: ''
    };
  },
  created() {
    this.fetchFirstEpisodeName();
  },
  methods: {
    async fetchFirstEpisodeName() {
      try {
        const firstEpisodeUrl = this.character.episode[0];
        const response = await axios.get(firstEpisodeUrl);
        this.firstEpisodeName = response.data.name;
      } catch (error) {
        console.error('Error fetching episode name:', error);
      }
    }
  }
}
</script>

<style scoped>

.character-card {
    width: 600px;
    height: 220px;
    display: flex;
    overflow: hidden;
    background: rgb(60, 62, 68);
    border-radius: 0.5rem;
    margin: 0.75rem;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}
.section {

    margin: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex: 1;


}

.character-card img {
    width: 229.2px;
    height: 220px;
    object-fit: cover;
}

.character-info {
    width: 370.8px;
    height: 196px;
    padding: 0.75rem;
    position: relative;
    color: rgb(255, 255, 255);
    display: flex;
    flex-direction: column;
    text-align: left;
}


.character-info h2,
.character-info span,
.character-info p {
  margin: 0;
  font-family: -apple-system,'BlinkMacSystemFont','Segoe UI','Roboto','Helvetica','Arial',sans-serif,'Apple Color Emoji','Segoe UI Emoji','Segoe UI Symbol';
}


.gray-label {
    color: rgb(158, 158, 158); /* gray color" */
}

@media (max-width: 40.625em) {
    .character-card {
        flex-direction: column;
        height: initial;
        width: 100%;
    }

    .character-card img {
        width: 100%;
    }
}


</style>


