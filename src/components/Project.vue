<template>
  <section class="projects-section">

    <!-- Header -->
    <div class="projects-header">
      <span class="projects-tag">My Work</span>
      <h1 class="projects-title">My <span class="accent">Projects</span></h1>
      <div class="projects-underline"></div>
      <p class="projects-sub">{{ projects.length }} projects built & shipped</p>
    </div>

    <!-- Grid -->
    <div class="projects-grid">
      <div
        v-for="(project, i) in paginatedProjects"
        :key="project.title"
        class="project-card"
        :style="{ animationDelay: `${i * 0.08}s` }"
      >
        <!-- Image -->
        <div class="project-img-wrap">
          <img :src="project.img" :alt="project.title" class="project-img" loading="lazy" />
          <div class="project-overlay">
            <a
              v-if="project.demo"
              :href="project.demo"
              target="_blank"
              rel="noopener noreferrer"
              class="overlay-btn"
              @click.stop
            >
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                <polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/>
              </svg>
              Live Demo
            </a>
            <span v-else class="overlay-label">Private Project</span>
          </div>
        </div>

        <!-- Body -->
        <div class="project-body">
          <div class="project-top">
            <span class="project-num">{{ String(startIndex + i + 1).padStart(2, '0') }}</span>
            <h3 class="project-title">{{ project.title }}</h3>
          </div>
          <p class="project-desc">{{ project.description }}</p>
          <div class="project-tags">
            <span v-for="tag in project.tags" :key="tag" class="project-tag">{{ tag }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Pagination -->
    <div class="projects-pagination">
      <button class="pg-btn" :disabled="currentPage === 1" @click="prevPage">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="15 18 9 12 15 6"/>
        </svg>
        Prev
      </button>

      <div class="pg-dots">
        <button
          v-for="p in totalPages"
          :key="p"
          class="pg-dot"
          :class="{ active: p === currentPage }"
          @click="goToPage(p)"
        ></button>
      </div>

      <button class="pg-btn" :disabled="currentPage === totalPages" @click="nextPage">
        Next
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="9 18 15 12 9 6"/>
        </svg>
      </button>
    </div>

  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

const ITEMS_PER_PAGE = 4

const projects = [
  {
    title: 'Hotel System',
    img: '/assets/hotel.png',
    description: 'Hotel management system for room booking and food ordering. Final year degree project.',
    tags: ['Laravel', 'Angular', 'Bootstrap', 'CoreUI'],
    demo: null
  },
  {
    title: 'Salary Management System',
    img: '/assets/Salary_management.png',
    description: 'This modern, high-fidelity payroll management dashboard showcases advanced data visualization and UI/UX design skills using real-time analytics. It demonstrates a professional ability to architect complex enterprise systems that balance intricate financial logic with a clean, user-centric interface.',
    tags: ['Spring Boot', 'React', 'Bootstrap'],
     demo: null
    // demo: 'https://spontaneous-lily-406fd6.netlify.app/'
  },
   {
    title: 'Chat App',
    img: '/assets/chat_app.png',
    description: 'Real-time chat between two persons. An ongoing project to implement a full-featured chat application.',
    tags: ['Laravel', 'Vue.js', 'Bootstrap'],
    demo: null
  },
  {
    title: 'Online Store',
    img: '/assets/online_store.png',
    description: 'IRA Enterprises offers a secure online shopping experience with verified sellers across the country, ensuring safe and prompt delivery of orders.',
    tags: ['Laravel', 'Angular', 'Bootstrap', 'Angular Material'],
    demo: null
  },
  {
    title: 'Blog System',
    img: '/assets/blog.png',
    description: 'An education blog platform to improve and share knowledge across communities.',
    tags: ['Bootstrap', 'OOP PHP'],
    demo: null
  },
  {
    title: 'Bus Booking System',
    img: '/assets/Bus_System.png',
    description: 'Bus booking system for seat reservation and ticket management.',
    tags: ['PHP', 'HTML/CSS', 'JavaScript'],
    demo: null
  },
  {
    title: 'Country & Weather App',
    img: '/assets/Cuntry_Wether_App.png',
    description: 'View country details and per-city weather information. ICET coursework project.',
    tags: ['Bootstrap', 'HTML/CSS', 'JavaScript'],
    demo: 'https://sahangeesara.github.io/CuntryWeb/'
  },
  {
    title: 'Movie & Meal Hub',
    img: '/assets/Movie_and_Meal_Hub.png',
    description: 'Browse movie reviews and meal information. ICET coursework project.',
    tags: ['Bootstrap', 'HTML/CSS', 'JavaScript'],
    demo: 'https://sahangeesara.github.io/Meal_OM_web_page/index.html'
  },
  {
    title: 'FashionRack POS System',
    img: '/assets/pos_system.png',
    description: 'Cashier login, order placement with or without customer, product & customer management saved to local storage. ICET coursework.',
    tags: ['Bootstrap', 'HTML/CSS', 'JavaScript', 'SweetAlert2'],
    demo: 'https://sahangeesara.github.io/Ch_Post_system/index.html'
  },
  {
    title: 'Tax Calculator System',
    img: '/assets/Tex_calculator.png',
    description: 'A tax calculation utility built as ICET coursework.',
    tags: ['Java'],
    demo: 'https://sahangeesara.github.io/Tax_Calculator_system/'
  }
]

const currentPage = ref(1)

const totalPages = computed(() => Math.ceil(projects.length / ITEMS_PER_PAGE))
const startIndex = computed(() => (currentPage.value - 1) * ITEMS_PER_PAGE)
const paginatedProjects = computed(() =>
  projects.slice(startIndex.value, startIndex.value + ITEMS_PER_PAGE)
)

function prevPage() { if (currentPage.value > 1) currentPage.value-- }
function nextPage() { if (currentPage.value < totalPages.value) currentPage.value++ }
function goToPage(p) { currentPage.value = p }
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

/* ── Section ── */
.projects-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 5rem;
  min-height: 50vh;
  box-sizing: border-box;
}

.projects-header {
  text-align: center;
  margin-bottom: 3.5rem;
}
.projects-tag {
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
.projects-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.accent { color: #e05c2a; }
.projects-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.projects-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* ── Grid ── */
.projects-grid {
  max-width: 1080px;
  margin: 0 auto 2.5rem;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.4rem;
}
@media (max-width: 720px) {
  .projects-grid { grid-template-columns: 1fr; }
}

/* ── Card ── */
.project-card {
  background: #161616;
  border: 1px solid #242424;
  border-radius: 18px;
  overflow: hidden;
  animation: fadeUp 0.5s ease both;
  transition: border-color 0.3s, transform 0.3s;
  display: flex;
  flex-direction: column;
}
.project-card:hover {
  border-color: rgba(224, 92, 42, 0.45);
  transform: translateY(-4px);
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(22px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Image — KEY FIX ── */
.project-img-wrap {
  position: relative;
  width: 100%;
  height: 200px;        /* fixed height — every card identical */
  overflow: hidden;
  background: #111;     /* fallback while image loads */
  flex-shrink: 0;
}
.project-img {
  width: 100%;
  height: 100%;
  object-fit: cover;    /* fills the box, crops evenly */
  object-position: top; /* show the top (UI) rather than cutting it off */
  display: block;
  transition: transform 0.4s ease;
}
.project-card:hover .project-img { transform: scale(1.05); }

.project-overlay {
  position: absolute;
  inset: 0;
  background: rgba(13, 13, 13, 0.78);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s;
}
.project-card:hover .project-overlay { opacity: 1; }

.overlay-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  background: #e05c2a;
  color: #fff;
  text-decoration: none;
  font-size: 0.82rem;
  font-weight: 600;
  padding: 0.55rem 1.1rem;
  border-radius: 100px;
  transition: background 0.2s, transform 0.2s;
}
.overlay-btn:hover {
  background: #c94f22;
  transform: scale(1.05);
}

.overlay-label {
  font-size: 0.78rem;
  color: #666;
  font-weight: 500;
  background: rgba(255,255,255,0.05);
  border: 1px solid #333;
  border-radius: 100px;
  padding: 0.4rem 1rem;
}

/* ── Body ── */
.project-body {
  padding: 1.25rem 1.3rem 1.3rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.project-top {
  display: flex;
  align-items: center;
  gap: 0.65rem;
}
.project-num {
  font-family: 'Sora', sans-serif;
  font-size: 0.7rem;
  font-weight: 700;
  color: #e05c2a;
  opacity: 0.6;
}
.project-title {
  font-family: 'Sora', sans-serif;
  font-size: 1rem;
  font-weight: 700;
  color: #f0f0f0;
  margin: 0;
}

.project-desc {
  font-size: 0.83rem;
  color: #777;
  line-height: 1.65;
  margin: 0;
  flex: 1;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
  margin-top: auto;
  padding-top: 0.5rem;
  border-top: 1px solid #1e1e1e;
}
.project-tag {
  font-size: 0.72rem;
  font-weight: 500;
  color: #888;
  background: #1c1c1c;
  border: 1px solid #2a2a2a;
  border-radius: 100px;
  padding: 0.2rem 0.65rem;
  transition: color 0.2s, border-color 0.2s;
}
.project-card:hover .project-tag {
  color: #e05c2a;
  border-color: rgba(224, 92, 42, 0.3);
}

/* ── Pagination ── */
.projects-pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  margin-bottom: 0.75rem;
}
.pg-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  background: #181818;
  border: 1px solid #2e2e2e;
  color: #ccc;
  border-radius: 10px;
  padding: 0.6rem 1.2rem;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s, border-color 0.2s, color 0.2s, transform 0.2s;
}
.pg-btn:hover:not(:disabled) {
  background: rgba(224, 92, 42, 0.12);
  border-color: rgba(224, 92, 42, 0.45);
  color: #e05c2a;
  transform: translateY(-1px);
}
.pg-btn:disabled { opacity: 0.3; cursor: not-allowed; }

.pg-dots {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.pg-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #333;
  border: none;
  cursor: pointer;
  transition: background 0.2s, transform 0.2s;
  padding: 0;
}
.pg-dot.active {
  background: #e05c2a;
  transform: scale(1.3);
}
.pg-info {
  text-align: center;
  font-size: 0.78rem;
  color: #555;
  margin: 0;
}
</style>