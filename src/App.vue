<script setup>
//Henter ref, så Vue kan holde øje med listen
import {ref} from "vue";

//Henter jobdata fra data.json
import data from "./data.json";

//Gemmer dataen i ref variable
const jobs = ref(data);

//Samler alle tags for ét job i én liste
function getTags (job) {
  // role og level er enkelte ord. languages og tools er lister, som bliver foldt ud med "..."
  return [job.role,job.level, ...job.languages, ...job.tools];
}

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
      <li v-for="job in jobs" :key="job.id" class="job-card" :class="{featured: job.featured}">
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
          <button type="button" class="tag">{{ tag }}</button>
        </li>
      </ul>

      </li>
    </ul>
   </main>

</template>
