<script setup>
//Henter ref og computed, så Vue kan holde øje med listen
import {ref, computed} from "vue";

//Henter jobdata fra data.json
import data from "./data.json";

//Gemmer dataen i ref variable
const jobs = ref(data);

//Samler alle tags for ét job i én liste
function getTags (job) {
  // role og level er enkelte ord. languages og tools er lister, som bliver foldt ud med "..."
  return [job.role,job.level, ...job.languages, ...job.tools];
}

// de tags brugeren har valgt at filtrere på, starter tom
const filters = ref([]);

//Tilføjer et tag til filtrene, hvis det ikke allerede er der
function addFilter(tag) {
  if(!filters.value.includes(tag)){
    filters.value.push(tag);
  }
}

//Listen af jobs, der skal vises. Regnes automatisk ud igen, når filtres ændres sig
const filteredJobs = computed(() => {
  //Beholder kun de jobs, der passer
  return jobs.value.filter((job) => {
    const tags = getTags(job);
    //Jobbet passer, hvis det har hvert eneste valgte filter
    return filters.value.every((filter) => tags.includes(filter));
  });
});

</script>

<template>
  <header class="site-header">
    <h1 class="visually-hidden">Job listings</h1>
  </header>
  <!-- Sidens hovedindhold -->
   <main>
    <!-- Viser jobs i en listevisning -->
    <ul class="job-list">
      <!-- Èt listepunkt pr. job, hvor id'et bruges som unik nøgle -->
      <li v-for="job in filteredJobs" :key="job.id" class="job-card" :class="{ featured: job.featured }">
        <img class="job-logo" :src="job.logo" :alt="job.company + ' logo'">
        
        <!-- Alt tekst omkring jobbet -->
        <div class="job-info">
          <div class="job-company">
            <p>{{ job.company }}</p>
            <!-- Vises kun hvis jobbet er nyt -->
            <span v-if="job.new" class="badge badge-new">New!</span>

            <!-- Vises kun hvis jobbet er featured -->
            <span v-if="job.featured" class="badge badge-featured">Featured</span>
          </div>
          <h2>{{ job.position }}</h2>
  
          <ul class="job-meta">
            <li>{{ job.postedAt }}</li>
            <li>{{ job.contract }}</li>
            <li>{{ job.location }}</li>
          </ul>
  
      </div>
      <ul class="job-tags">
        <li v-for="tag in getTags(job)" :key="tag">
          <!-- Klik tilføjer tagget som filter -->
          <button type="button" class="tag" @click="addFilter(tag)">{{ tag }}</button>
        </li>
      </ul>

      </li>
    </ul>
   </main>

</template>
