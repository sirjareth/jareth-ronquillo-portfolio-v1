<script setup>
  import Navbar from './components/Navbar.vue';
  import Landing from './components/Landing.vue';
  import Projects from './components/Projects.vue';
  import TechStack from './components/TechStack.vue';
  import Contact from './components/Contact.vue';
  import FooterComponent from './components/FooterComponent.vue';
</script>

<template>
  <Navbar/>
  <div class="container-fluid px-0">
    <Landing/>
    <Projects/>
    <TechStack/>
    <Contact/>
    <FooterComponent/>
  </div>
  
</template>
