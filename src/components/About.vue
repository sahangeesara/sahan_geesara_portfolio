<template>
  <section class="about-section">

    <!-- Header -->
    <div class="section-header">
      <span class="section-tag">Get To Know Me</span>
      <h1 class="section-title">About <span class="accent">Me</span></h1>
      <div class="title-underline"></div>
    </div>

    <!-- Main Content -->
    <div class="about-container">

      <!-- Left: Image -->
      <div class="image-col" :class="{ visible: imageVisible }" ref="imageRef">
        <div class="image-wrapper">
          <div class="image-glow"></div>
          <img src="/assets/ani.png" alt="Sahan Geesara" class="profile-img" />
          <div class="experience-badge">
            <span class="badge-number">6+</span>
            <span class="badge-label">Months Exp.</span>
          </div>
        </div>
      </div>

      <!-- Right: Info -->
      <div class="info-col" :class="{ visible: infoVisible }" ref="infoRef">

        <!-- Stats Cards -->
        <div class="stats-grid">
          <div
            v-for="(stat, i) in stats"
            :key="i"
            class="stat-card"
            :class="{ hovered: hoveredStat === i }"
            @mouseenter="hoveredStat = i"
            @mouseleave="hoveredStat = null"
          >
            <div class="stat-icon" v-html="stat.icon"></div>
            <h3 class="stat-value">{{ stat.value }}</h3>
            <p class="stat-label">{{ stat.label }}</p>
            <div class="stat-accent"></div>
          </div>
        </div>

        <!-- Bio Text -->
        <div class="bio-text">
          <p>
            I'm a <strong>Passionate Fullstack Developer</strong> with a knack for crafting
            innovative web solutions. Fluent in both front-end and back-end technologies,
            I bring a holistic approach to development.
          </p>
        </div>

        <!-- Feature Bullets -->
        <div class="feature-list">
          <div v-for="(feature, i) in features" :key="i" class="feature-item">
            <div class="feature-dot"></div>
            <p>{{ feature }}</p>
          </div>
        </div>

        <!-- Skills Pills -->
        <div class="skills-row">
          <span v-for="skill in skills" :key="skill" class="skill-pill">{{ skill }}</span>
        </div>

        <!-- CTA Buttons -->
        <div class="cta-row">
          <button class="btn-primary" @click="downloadFile">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
              <polyline points="7 10 12 15 17 10"/>
              <line x1="12" y1="15" x2="12" y2="3"/>
            </svg>
            Download Resume
          </button>
          <button class="btn-outline" @click="scrollToContact">
            Let's Talk
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="5" y1="12" x2="19" y2="12"/>
              <polyline points="12 5 19 12 12 19"/>
            </svg>
          </button>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const imageRef = ref(null)
const infoRef  = ref(null)
const imageVisible = ref(false)
const infoVisible  = ref(false)
const hoveredStat  = ref(null)

const stats = [
  {
    value: '6+',
    label: 'Months Working',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/></svg>`
  },
  {
    value: '2+',
    label: 'Projects Done',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></svg>`
  },
  {
    value: '24/7',
    label: 'Support',
    icon: `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 18v-6a9 9 0 0 1 18 0v6"/><path d="M21 19a2 2 0 0 1-2 2h-1a2 2 0 0 1-2-2v-3a2 2 0 0 1 2-2h3z"/><path d="M3 19a2 2 0 0 0 2 2h1a2 2 0 0 0 2-2v-3a2 2 0 0 0-2-2H3z"/></svg>`
  }
]

const features = [
  'Crafting innovative, pixel-perfect web solutions from concept to deployment.',
  'Fluent in Vue, React, Node.js, and modern databases with clean, maintainable code.',
  'Dedicated to staying ahead of the curve with the latest technologies and best practices.'
]

const skills = ['Vue.js', 'React', 'Node.js', 'Laravel', 'MySQL', 'Tailwind CSS']

function downloadFile() {
  const link = document.createElement('a')
  link.href = '/assets/Sahan_Geesara_CV.pdf'
  link.download = 'Sahan_Geesara_CV.pdf'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}

function scrollToContact() {
  const contact = document.querySelector('#contact')
  if (contact) contact.scrollIntoView({ behavior: 'smooth' })
}

let observer

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(entry => {
        if (entry.target === imageRef.value && entry.isIntersecting) imageVisible.value = true
        if (entry.target === infoRef.value  && entry.isIntersecting) infoVisible.value  = true
      })
    },
    { threshold: 0.1 }
  )
  if (imageRef.value) observer.observe(imageRef.value)
  if (infoRef.value)  observer.observe(infoRef.value)
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

.about-section {
  font-family: 'DM Sans', sans-serif;
  padding: 6rem 1.5rem;
  background: #0d0d0d;
  color: #f0f0f0;
  min-height: 100vh;
  box-sizing: border-box;
}

/* Header */
.section-header {
  text-align: center;
  margin-bottom: 4rem;
}
.section-tag {
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
.section-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 1rem;
}
.accent { color: #e05c2a; }
.title-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto;
}

/* Layout */
.about-container {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 4rem;
  align-items: center;
}
@media (max-width: 768px) {
  .about-container {
    grid-template-columns: 1fr;
    gap: 2.5rem;
  }
  .about-section { padding: 4rem 1.25rem; }
}

/* Image */
.image-col {
  opacity: 0;
  transform: translateX(-36px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}
.image-col.visible { opacity: 1; transform: translateX(0); }

.image-wrapper {
  position: relative;
  max-width: 380px;
  margin: 0 auto;
}
.image-glow {
  position: absolute;
  inset: -16px;
  border-radius: 50%;
  background: radial-gradient(ellipse, rgba(224,92,42,0.2) 0%, transparent 70%);
  z-index: 0;
}
.profile-img {
  position: relative;
  z-index: 1;
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 50%;
  border: 4px solid rgba(224, 92, 42, 0.55);
  display: block;
  transition: transform 0.4s ease;
}
.profile-img:hover { transform: scale(1.025); }

.experience-badge {
  position: absolute;
  bottom: 16px;
  right: -8px;
  z-index: 2;
  background: #e05c2a;
  border-radius: 14px;
  padding: 0.6rem 1rem;
  text-align: center;
  box-shadow: 0 6px 20px rgba(224, 92, 42, 0.4);
}
.badge-number {
  display: block;
  font-family: 'Sora', sans-serif;
  font-size: 1.3rem;
  font-weight: 700;
  color: #fff;
  line-height: 1;
}
.badge-label {
  display: block;
  font-size: 0.65rem;
  color: rgba(255,255,255,0.85);
  letter-spacing: 0.04em;
  margin-top: 2px;
}

/* Info column */
.info-col {
  opacity: 0;
  transform: translateX(36px);
  transition: opacity 0.7s ease 0.15s, transform 0.7s ease 0.15s;
}
.info-col.visible { opacity: 1; transform: translateX(0); }

/* Stats */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.9rem;
  margin-bottom: 1.8rem;
}
@media (max-width: 480px) {
  .stats-grid { grid-template-columns: 1fr; }
}

.stat-card {
  position: relative;
  overflow: hidden;
  background: #181818;
  border: 1px solid #272727;
  border-radius: 16px;
  padding: 1.1rem 1rem;
  cursor: default;
  transition: border-color 0.3s, transform 0.3s;
}
.stat-card.hovered {
  border-color: rgba(224, 92, 42, 0.5);
  transform: translateY(-4px);
}
.stat-icon {
  color: #e05c2a;
  margin-bottom: 0.5rem;
  line-height: 0;
}
.stat-value {
  font-family: 'Sora', sans-serif;
  font-size: 1.5rem;
  font-weight: 700;
  color: #f8f8f8;
  margin: 0 0 2px;
  line-height: 1;
}
.stat-label {
  font-size: 0.73rem;
  color: #777;
  margin: 0;
  font-weight: 500;
}
.stat-accent {
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  opacity: 0;
  transition: opacity 0.3s;
}
.stat-card.hovered .stat-accent { opacity: 1; }

/* Bio */
.bio-text {
  margin-bottom: 1.4rem;
  color: #999;
  font-size: 0.93rem;
  line-height: 1.75;
}
.bio-text strong { color: #eee; font-weight: 600; }

/* Features */
.feature-list {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  margin-bottom: 1.6rem;
}
.feature-item {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
}
.feature-dot {
  width: 8px;
  height: 8px;
  min-width: 8px;
  background: #e05c2a;
  border-radius: 50%;
  margin-top: 6px;
}
.feature-item p {
  color: #aaa;
  font-size: 0.87rem;
  line-height: 1.65;
  margin: 0;
}

/* Skills */
.skills-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 2rem;
}
.skill-pill {
  background: #1c1c1c;
  border: 1px solid #2e2e2e;
  color: #bbb;
  font-size: 0.78rem;
  font-weight: 500;
  padding: 0.3rem 0.85rem;
  border-radius: 100px;
  transition: background 0.2s, border-color 0.2s, color 0.2s;
  cursor: default;
}
.skill-pill:hover {
  background: rgba(224, 92, 42, 0.1);
  border-color: rgba(224, 92, 42, 0.45);
  color: #e05c2a;
}

/* Buttons */
.cta-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
}
.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: #e05c2a;
  color: #fff;
  border: none;
  border-radius: 12px;
  padding: 0.85rem 1.7rem;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.25s, transform 0.2s, box-shadow 0.25s;
  box-shadow: 0 4px 16px rgba(224, 92, 42, 0.35);
}
.btn-primary:hover {
  background: #c94f22;
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(224, 92, 42, 0.45);
}
.btn-primary:active { transform: translateY(0); }

.btn-outline {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: transparent;
  color: #e05c2a;
  border: 1.5px solid rgba(224, 92, 42, 0.45);
  border-radius: 12px;
  padding: 0.85rem 1.7rem;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s, border-color 0.2s, transform 0.2s;
}
.btn-outline:hover {
  background: rgba(224, 92, 42, 0.1);
  border-color: #e05c2a;
  transform: translateY(-2px);
}
.btn-outline:active { transform: translateY(0); }
</style>