<template>
  <section class="skills-section">

    <!-- Header -->
    <div class="skills-header">
      <span class="skills-tag">What I Know</span>
      <h1 class="skills-title">My <span class="accent">Skills</span></h1>
      <div class="skills-underline"></div>
      <p class="skills-sub">Technologies & tools I work with</p>
    </div>

    <!-- Cards Grid -->
    <div class="skills-grid">
      <div
        v-for="(category, ci) in skillCategories"
        :key="category.title"
        class="skill-card"
        :style="{ animationDelay: `${ci * 0.1}s` }"
      >
        <div class="card-accent"></div>

        <!-- Card Header -->
        <div class="card-head">
          <div class="card-icon" v-html="category.icon"></div>
          <h3 class="card-title">{{ category.title }}</h3>
          <span class="card-count">{{ category.skills.length }} skills</span>
        </div>

        <!-- Skill Items -->
        <ul class="skill-list">
          <li
            v-for="(skill, si) in category.skills"
            :key="skill"
            class="skill-item"
            :style="{ animationDelay: `${ci * 0.1 + si * 0.07}s` }"
          >
            <span class="skill-dot"></span>
            <span class="skill-name">{{ skill }}</span>
          </li>
        </ul>
      </div>
    </div>

  </section>
</template>

<script setup>
const skillCategories = [
  {
    title: 'Frontend',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>`,
    skills: ['Vue.js', 'Angular', 'React', 'HTML / CSS', 'JavaScript']
  },
  {
    title: 'Backend',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><line x1="6" y1="6" x2="6.01" y2="6"/><line x1="6" y1="18" x2="6.01" y2="18"/></svg>`,
    skills: ['PHP', 'Laravel', 'Java', 'Spring Boot']
  },
  {
    title: 'Database',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"/><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"/><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"/></svg>`,
    skills: ['MySQL', 'MsSQL']
  },
  {
    title: 'Tools',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>`,
    skills: ['GitHub', 'GitLab', 'Postman', 'Jira', 'IntelliJ', 'PhpStorm']
  }
]
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

.skills-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 5rem;
  min-height: 50vh;
  box-sizing: border-box;
}

/* Header */
.skills-header {
  text-align: center;
  margin-bottom: 3.5rem;
}
.skills-tag {
  display: inline-block;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.1);
  border: 1px solid rgba(224, 92, 42, 0.3);
  border-radius: 100px;
  padding: 0.35rem 1rem;
  margin-bottom: 1rem;
}
.skills-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.accent { color: #e05c2a; }
.skills-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.skills-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* Grid */
.skills-grid {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.4rem;
}
@media (max-width: 700px) {
  .skills-grid { grid-template-columns: 1fr; }
}

/* Card */
.skill-card {
  position: relative;
  background: #161616;
  border: 1px solid #242424;
  border-radius: 18px;
  padding: 1.6rem 1.5rem 1.5rem;
  overflow: hidden;
  animation: fadeUp 0.5s ease both;
  transition: border-color 0.3s, transform 0.3s;
}
.skill-card:hover {
  border-color: rgba(224, 92, 42, 0.45);
  transform: translateY(-4px);
}
.skill-card:hover .card-accent { opacity: 1; }

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(22px); }
  to   { opacity: 1; transform: translateY(0); }
}

.card-accent {
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  opacity: 0;
  transition: opacity 0.3s;
}

/* Card head */
.card-head {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 1.3rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #222;
}
.card-icon {
  color: #e05c2a;
  line-height: 0;
  flex-shrink: 0;
}
.card-title {
  font-family: 'Sora', sans-serif;
  font-size: 1.05rem;
  font-weight: 700;
  color: #f0f0f0;
  margin: 0;
  flex: 1;
}
.card-count {
  font-size: 0.7rem;
  color: #444;
  font-weight: 500;
  background: #1e1e1e;
  border: 1px solid #2a2a2a;
  border-radius: 100px;
  padding: 0.2rem 0.65rem;
  white-space: nowrap;
}

/* Skill list */
.skill-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 0.65rem;
}

.skill-item {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  background: #1c1c1c;
  border: 1px solid #2a2a2a;
  border-radius: 100px;
  padding: 0.4rem 0.9rem;
  animation: fadeUp 0.4s ease both;
  transition: background 0.2s, border-color 0.2s, color 0.2s;
  cursor: default;
}
.skill-item:hover {
  background: rgba(224, 92, 42, 0.1);
  border-color: rgba(224, 92, 42, 0.45);
}
.skill-item:hover .skill-dot {
  background: #f5a623;
}

.skill-dot {
  width: 6px;
  height: 6px;
  background: #e05c2a;
  border-radius: 50%;
  flex-shrink: 0;
  transition: background 0.2s;
}
.skill-name {
  font-size: 0.84rem;
  color: #bbb;
  font-weight: 500;
  transition: color 0.2s;
}
.skill-item:hover .skill-name {
  color: #e05c2a;
}
</style>