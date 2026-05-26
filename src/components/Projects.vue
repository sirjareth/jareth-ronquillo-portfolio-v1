<template>
  <div class="row p-3 p-md-5" id="projects">
    <!-- Section Title -->
    <div class="col-12 mb-4">
      <p class="section-label">Portfolio</p>
      <h1>
        <span class="title-white">My</span>
        <span class="title-teal">Projects</span>
      </h1>
    </div>

    <!-- Render project chunks -->
    <div
      v-for="(group, index) in chunkedProjects"
      :key="index"
      class="row justify-content-center my-4"
    >
      <div
        v-for="project in group"
        :key="project.id"
        class="col-12 col-md-6 col-lg-4 mb-4"
      >
        <ProjectCard :project="project" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import ProjectCard from './ProjectCard.vue';
import projects from '../data/projects.json';

const chunkSize = 3;

const chunkedProjects = computed(() => {
  const chunks = [];
  for (let i = 0; i < projects.length; i += chunkSize) {
    chunks.push(projects.slice(i, i + chunkSize));
  }
  return chunks;
});
</script>
