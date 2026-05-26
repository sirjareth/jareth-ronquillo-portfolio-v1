<template>
  <div class="card h-100 project-card">
    <img :src="project.image" class="card-img-top" alt="E-commerce API Documentation">
    <div class="card-body">
      <h5>{{ project.title }}</h5>
      <p class="card-text">{{ project.description }}</p>
    </div>
  </div>
</template>

<script setup>
  import { defineProps } from 'vue';

  defineProps({
    project: Object
  })
</script>