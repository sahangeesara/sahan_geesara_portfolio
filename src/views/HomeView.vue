<script setup>
import Project from "../components/Project.vue";
import Skills from "../components/Skills.vue";
import Education from "../components/Education.vue";
import WorkExperience from "../components/WorkExperience.vue";
import Contact from "../components/Contact.vue";
import About from "../components/About.vue";
import Welcome from "../components/Welcome.vue";
import Certificates from "@/components/Certificates.vue";
</script>

<template>
  <div class="home">
    <section id="home"><Welcome /></section>
    <section id="about"><About /></section>
    <section id="skills"><Skills /></section>
    <section id="project"><Project /></section>
    <section id="education"><Education /></section>
    <section id="certificate"><Certificates /></section>
    <section id="experience"><WorkExperience /></section>
    <section id="contact"><Contact /></section>
  </div>
</template>

<style scoped>
.home {
  background-color: #0d0d0d;
  min-height: 50vh;
  width: 100%;
}

/* Remove any section gaps / white flashes between components */
section {
  background-color: #0d0d0d;
}
</style>