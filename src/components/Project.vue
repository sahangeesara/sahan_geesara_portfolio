<template>
  <section class="projects-section" aria-label="Projects section">

    <!-- Header -->
    <div class="projects-header">
      <span class="projects-tag">My Work</span>
      <h1 class="projects-title">My <span class="accent">Projects</span></h1>
      <div class="projects-underline"></div>
      <p class="projects-sub">{{ filteredProjects.length }} of {{ projects.length }} projects</p>
    </div>

    <div class="projects-toolbar" aria-label="Project filters">
      <label class="sr-only" for="project-filter">Filter by demo availability</label>
      <select id="project-filter" v-model="demoFilter" class="toolbar-select">
        <option value="all">All projects</option>
        <option value="demo">With live demo</option>
        <option value="private">Without demo</option>
      </select>

      <button type="button" class="toolbar-reset" @click="resetFilters">Reset</button>
    </div>

    <!-- Grid -->
    <div class="projects-grid" v-if="paginatedProjects.length">
      <div
        v-for="(project, i) in paginatedProjects"
        :key="project.title"
        class="project-card"
        :style="{ animationDelay: `${i * 0.08}s` }"
      >
        <!-- Image -->
        <button
          type="button"
          class="project-img-wrap"
          :aria-label="`Open larger preview for ${project.title}`"
          @click="openProjectViewer(project)"
        >
          <img :src="getProjectImage(project)" :alt="project.title" class="project-img" loading="lazy" />

          <span v-if="hasMultipleImages(project)" class="img-badge">
            {{ getProjectMedia(project).length }} photos
          </span>

          <span class="project-overlay">
            <span class="overlay-label overlay-cta">Click to view larger</span>
          </span>
        </button>

        <!-- Body -->
        <div class="project-body">
          <div class="project-top">
            <span class="project-num">{{ String(startIndex + i + 1).padStart(2, '0') }}</span>
            <h3 class="project-title">{{ project.title }}</h3>
          </div>
          <p class="project-desc">{{ project.description }}</p>
          <div class="project-actions">
            <button type="button" class="action-btn primary" @click="openProjectViewer(project)">
              View image
            </button>
            <a
              v-if="project.demo"
              :href="project.demo"
              target="_blank"
              rel="noopener noreferrer"
              class="action-btn secondary"
              :aria-label="`Open live demo for ${project.title}`"
            >
              Live demo
            </a>
            <span v-else class="action-chip">No live demo</span>
          </div>
          <div class="project-tags">
            <span v-for="tag in project.tags" :key="tag" class="project-tag">{{ tag }}</span>
          </div>
        </div>
      </div>
    </div>

    <div v-else class="projects-empty" role="status" aria-live="polite">
      <h3>No matching projects</h3>
      <p>Try changing the demo filter.</p>
    </div>

    <!-- Pagination -->
    <nav class="projects-pagination" aria-label="Projects pagination">
      <button
        type="button"
        class="pg-btn"
        :disabled="currentPage === 1"
        aria-label="Go to previous project page"
        @click="prevPage"
      >
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="15 18 9 12 15 6"/>
        </svg>
        Prev
      </button>

      <fieldset class="pg-dots">
        <legend class="sr-only">Select project page</legend>
        <button
          v-for="p in totalPages"
          :key="p"
          type="button"
          class="pg-dot"
          :class="{ active: p === currentPage }"
          :aria-label="`Go to page ${p}`"
          :aria-current="p === currentPage ? 'page' : undefined"
          @click="goToPage(p)"
        ></button>
      </fieldset>

      <button
        type="button"
        class="pg-btn"
        :disabled="currentPage === totalPages"
        aria-label="Go to next project page"
        @click="nextPage"
      >
        Next
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="9 18 15 12 9 6"/>
        </svg>
      </button>
    </nav>


    <div v-if="activeProject" class="viewer-backdrop" @click.self="closeProjectViewer">
      <dialog open class="viewer-panel" :aria-label="`Project preview for ${activeProject.title}`" @cancel.prevent="closeProjectViewer">
        <div class="viewer-header">
          <div>
            <p class="viewer-kicker">Project preview</p>
            <h3 class="viewer-title">{{ activeProject.title }}</h3>
          </div>
          <!-- Removed alignment controls -->
          <button type="button" class="viewer-close" aria-label="Close preview" @click="closeProjectViewer">×</button>
        </div>

        <div class="viewer-media">
          <button
            v-if="hasMultipleImages(activeProject)"
            type="button"
            class="viewer-nav viewer-prev"
            aria-label="Previous image"
            @click="stepActiveViewer(-1)"
          >
            ‹
          </button>

          <img
            :src="viewerImageSrc"
            :alt="activeProject.title"
            class="viewer-img"
            @error="imageLoadError = true"
            v-if="!imageLoadError"
          />
          <div v-else class="viewer-img-error">
            <span>Image not available</span>
          </div>

          <button
            v-if="hasMultipleImages(activeProject)"
            type="button"
            class="viewer-nav viewer-next"
            aria-label="Next image"
            @click="stepActiveViewer(1)"
          >
            ›
          </button>
        </div>

        <div class="viewer-meta">
          <p class="viewer-desc">{{ activeProject.description }}</p>
          <span class="viewer-count">{{ viewerPositionLabel }}</span>
        </div>

        <div v-if="hasMultipleImages(activeProject)" class="viewer-thumbs" aria-label="Project images">
          <button
            v-for="(img, idx) in viewerMediaItems"
            :key="img"
            type="button"
            class="viewer-thumb"
            :class="{ active: idx === activeImageIndex }"
            :aria-label="`Show image ${idx + 1}`"
            @click="setActiveViewerImage(idx)"
          >
            <img :src="img" :alt="`${activeProject.title} thumbnail ${idx + 1}`" />
          </button>
        </div>

        <div class="viewer-tags">
          <span v-for="tag in activeProject.tags" :key="tag" class="viewer-tag">{{ tag }}</span>
        </div>
      </dialog>
    </div>

  </section>
</template>

<script setup>
import { ref, computed, watch, onUnmounted } from 'vue'

defineOptions({ name: 'ProjectSection' })

const ITEMS_PER_PAGE = 4

const projects = [
  {
    title: 'Hotel System',
    img: '/assets/hotel.png',
    description: 'Developed an integrated Hotel Management System to modernize operations by automating manual processes related to booking, guest records, billing, and service coordination, leading to improved efficiency, data accuracy, and faster decision-making. This is my campus final year project.',
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
    description: 'I have experience in PHP programming and web application development using Laravel, Vue JS, WebSocket (Pusher),\n' +
        'and Bootstrap, with a focus on real-time chat systems, and I use tools like IntelliJ IDEA and PhpStorm for coding and\n' +
        'project management. I have developed a chat application that allows users to register, log in, and engage in real-time conversations. The backend is built with Laravel, which handles user authentication, message storage, and WebSocket integration using Pusher for real-time communication. The frontend is developed with Vue JS and styled with Bootstrap, providing a responsive and user-friendly interface for seamless chatting experiences.',
    tags: ['Laravel', 'Vue.js', 'Bootstrap'],
    demo: null
  },
  {
    title: 'Online Store',
    img: '/assets/online_store.png',
    images: ['/assets/online_store.png', '/assets/online_store2.png','/assets/online_store3.png','/assets/online_store4.png','/assets/online_store5.png'],
    description: 'This project is an online store platform built with Angular (frontend) and Laravel (backend). It allows users to register, log in, and shop for products such as phones, vehicles, and laptops. The backend provides secure authentication using JWT, manages user and product data, and exposes RESTful APIs for the frontend. After login, users can view and update their profile details, manage their cart, and complete purchases. The system also includes features for password reset, contact forms, and profile management, making it a comprehensive solution for an e-commerce application.',
    tags: ['Laravel', 'Angular', 'Bootstrap', 'Angular Material'],
    demo: null
  },
  {
    title: 'Education Blog Platform',
    img: '/assets/blog1.png',
    images: ['/assets/blog1.png', '/assets/blog2.png'],
    description: 'This project is a PHP-based blog system that allows users to create, view, edit, and delete blog posts with images and descriptions. It features user authentication, an admin panel, and a responsive design. The system supports AI-generated post descriptions, enforces required fields for posts, and includes pagination for improved usability. The codebase is organized with controllers, components, and configuration files, making it easy to maintain and extend.',
    tags: ['Bootstrap', 'PHP', 'MySQL'],
    demo: null
  },
  {
    title: 'Bus Booking System',
    img: '/assets/Bus_System.png',
    description: 'This Bus Booking System is a web-based application designed to streamline the management and reservation of bus seats for both administrators and customers. Administrators and agents can manage customer records, bus schedules, and generate reports, while regular users can view their personal profiles and book seats. The system features clear role-based access, ensuring that only authorized users can access sensitive management functions, and provides a user-friendly interface for efficient booking and administration..',
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
const demoFilter = ref('all')
const projectMediaIndexes = ref({})
const activeProject = ref(null)
const activeImageIndex = ref(0)
// Removed: const imageAlignment = ref('center')

const FALLBACK_IMAGE = '/assets/placeholder.png'
const viewerImageSrc = computed(() => {
  if (!activeProject.value) return FALLBACK_IMAGE
  const media = viewerMediaItems.value
  return media[activeImageIndex.value] || activeProject.value.img || FALLBACK_IMAGE
})

const imageLoadError = ref(false)

const filteredProjects = computed(() => {
  return projects.filter((project) => {
    const matchesDemo =
      demoFilter.value === 'all' ||
      (demoFilter.value === 'demo' && Boolean(project.demo)) ||
      (demoFilter.value === 'private' && !project.demo)

    return matchesDemo
  })
})

const totalPages = computed(() => Math.max(1, Math.ceil(filteredProjects.value.length / ITEMS_PER_PAGE)))
const startIndex = computed(() => (currentPage.value - 1) * ITEMS_PER_PAGE)
const paginatedProjects = computed(() =>
  filteredProjects.value.slice(startIndex.value, startIndex.value + ITEMS_PER_PAGE)
)
watch(demoFilter, () => {
  currentPage.value = 1
})

watch(totalPages, (pages) => {
  if (currentPage.value > pages) {
    currentPage.value = pages
  }
})

function setPage(page) {
  currentPage.value = Math.min(Math.max(page, 1), totalPages.value)
}

function resetFilters() {
  demoFilter.value = 'all'
}

function getProjectMedia(project) {
  if (!project) return []

  const media = Array.isArray(project.images) && project.images.length
    ? project.images
    : [project.img]

  return [...new Set(media.filter(Boolean))]
}

function hasMultipleImages(project) {
  return getProjectMedia(project).length > 1
}

function getProjectImageIndex(project) {
  if (!hasMultipleImages(project)) return 0
  const media = getProjectMedia(project)
  const stored = projectMediaIndexes.value[project.title] || 0
  return Math.min(Math.max(stored, 0), media.length - 1)
}

function getProjectImage(project) {
  if (!hasMultipleImages(project)) return project.img
  const media = getProjectMedia(project)
  return media[getProjectImageIndex(project)] || project.img
}

function setProjectImageIndex(project, index) {
  if (!hasMultipleImages(project)) return
  const total = getProjectMedia(project).length
  const nextIndex = (index + total) % total
  projectMediaIndexes.value = { ...projectMediaIndexes.value, [project.title]: nextIndex }
}

function openProjectViewer(project) {
  activeProject.value = project
  activeImageIndex.value = getProjectImageIndex(project)
}

function closeProjectViewer() {
  activeProject.value = null
}

function setActiveViewerImage(index) {
  if (!activeProject.value) return
  const media = viewerMediaItems.value
  if (!media.length) return
  activeImageIndex.value = Math.min(Math.max(index, 0), media.length - 1)
  setProjectImageIndex(activeProject.value, index)
}

function stepActiveViewer(step) {
  if (!activeProject.value || !hasMultipleImages(activeProject.value)) return
  setActiveViewerImage(activeImageIndex.value + step)
}

const viewerMediaItems = computed(() => getProjectMedia(activeProject.value))

/* Removed alignment logic:
const objectPositionMap = {
  center: 'center center',
  top: 'top center',
  bottom: 'bottom center',
  left: 'center left',
  right: 'center right',
}

const viewerObjectPosition = computed(() => objectPositionMap[imageAlignment.value] || 'center center')
*/


const viewerPositionLabel = computed(() => {
  if (!activeProject.value) return ''
  if (!hasMultipleImages(activeProject.value)) return '1 image'
  return `${activeImageIndex.value + 1} / ${viewerMediaItems.value.length}`
})

function onKeydown(e) {
  if (!activeProject.value) return
  if (e.key === 'Escape') closeProjectViewer()
  if (e.key === 'ArrowLeft') stepActiveViewer(-1)
  if (e.key === 'ArrowRight') stepActiveViewer(1)
}

function prevPage() { setPage(currentPage.value - 1) }
function nextPage() { setPage(currentPage.value + 1) }
function goToPage(page) { setPage(page) }

watch(activeProject, (project) => {
  document.body.style.overflow = project ? 'hidden' : ''
})

onUnmounted(() => {
  document.body.style.overflow = ''
})

if (typeof window !== 'undefined') {
  window.addEventListener('keydown', onKeydown)
  onUnmounted(() => {
    window.removeEventListener('keydown', onKeydown)
  })
}
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
  margin-bottom: 2rem;
}
.projects-tag {
  display: inline-block;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.16);
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
  color: #8b8b8b;
  font-size: 0.88rem;
  margin: 0;
}

.projects-toolbar {
  max-width: 1080px;
  margin: 0 auto 1.4rem;
  display: grid;
  grid-template-columns: minmax(220px, 260px) auto;
  gap: 0.75rem;
  justify-content: center;
}
.toolbar-select,
.toolbar-reset {
  border: 1px solid #323232;
  background: #171717;
  color: #f0f0f0;
  border-radius: 10px;
  min-height: 42px;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
}
.toolbar-select {
  padding: 0.6rem 0.8rem;
  cursor: pointer;
}
.toolbar-reset {
  padding: 0.55rem 1rem;
  cursor: pointer;
  font-weight: 600;
}
.toolbar-reset:hover {
  border-color: rgba(224, 92, 42, 0.5);
  color: #ff9a66;
}
.toolbar-select:focus-visible,
.toolbar-reset:focus-visible {
  outline: 2px solid rgba(224, 92, 42, 0.8);
  outline-offset: 2px;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
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
  .projects-toolbar {
    grid-template-columns: 1fr;
  }
  .projects-grid { grid-template-columns: 1fr; }
}

/* ── Card ── */
.project-card {
  background: #141414;
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

.project-card:focus-within {
  border-color: rgba(224, 92, 42, 0.55);
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(22px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Image — KEY FIX ── */
.project-img-wrap {
  appearance: none;
  border: 0;
  padding: 0;
  position: relative;
  width: 100%;
  height: 220px;
  max-width: 100%;
  max-height: 220px;
  overflow: hidden;
  background: #111;
  color: inherit;
  flex-shrink: 0;
  cursor: zoom-in;
  outline: none;
  display: block;
  text-align: inherit;
}
.project-img {
  width: 100%;
  height: 100%;
  max-width: 100%;
  max-height: 220px;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: transform 0.4s ease;
}
.project-card:hover .project-img { transform: scale(1.04); }
.project-img-wrap:focus-visible {
  box-shadow: inset 0 0 0 2px rgba(224, 92, 42, 0.85);
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(13, 13, 13, 0.08), rgba(13, 13, 13, 0.7));
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 1;
  transition: opacity 0.3s;
}
.overlay-cta {
  backdrop-filter: blur(8px);
}

.img-badge {
  position: absolute;
  left: 0.8rem;
  top: 0.8rem;
  z-index: 2;
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  background: rgba(13, 13, 13, 0.75);
  border: 1px solid rgba(255, 255, 255, 0.14);
  border-radius: 999px;
  padding: 0.38rem 0.7rem;
  color: #f3f3f3;
  font-size: 0.72rem;
  font-weight: 600;
}

.project-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 0.55rem;
  margin-top: 0.2rem;
}
.action-btn,
.action-chip {
  min-height: 38px;
  border-radius: 12px;
  font-size: 0.82rem;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.55rem 0.9rem;
}
.action-btn {
  text-decoration: none;
  border: 1px solid transparent;
  transition: transform 0.2s, border-color 0.2s, background 0.2s;
}
.action-btn:hover {
  transform: translateY(-1px);
}
.action-btn.primary {
  background: linear-gradient(135deg, #e05c2a, #f07a3a);
  color: #fff;
}
.action-btn.secondary {
  background: #181818;
  color: #dedede;
  border-color: #303030;
}
.action-chip {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid #2d2d2d;
  color: #8b8b8b;
}

.overlay-label {
  font-size: 0.78rem;
  color: #f5f5f5;
  font-weight: 500;
  background: rgba(13, 13, 13, 0.55);
  border: 1px solid rgba(255, 255, 255, 0.16);
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
  color: #b4b4b4;
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

.projects-empty {
  max-width: 1080px;
  margin: 0 auto 2.2rem;
  background: #141414;
  border: 1px solid #2b2b2b;
  border-radius: 14px;
  padding: 1.5rem;
  text-align: center;
}
.projects-empty h3 {
  margin: 0 0 0.35rem;
  color: #f0f0f0;
  font-size: 1rem;
}
.projects-empty p {
  margin: 0;
  color: #a8a8a8;
  font-size: 0.86rem;
}

/* ── Pagination ── */
.projects-pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1.5rem;
  margin-bottom: 0.5rem;
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
.pg-btn:focus-visible,
.pg-dot:focus-visible {
  outline: 2px solid rgba(224, 92, 42, 0.8);
  outline-offset: 2px;
}

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

fieldset.pg-dots {
  border: 0;
  margin: 0;
  padding: 0;
  min-inline-size: 0;
}

/* ── Viewer ── */
.viewer-backdrop {
  position: fixed;
  inset: 0;
  z-index: 60;
  background: rgba(0, 0, 0, 0.82);
  backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.85rem 1rem 1rem;
  overflow-y: auto;
}
.viewer-panel {
  width: min(980px, 98vw);
  max-height: calc(100vh - 2.5rem);
  overflow: auto;
  background: #121212;
  border: 1px solid #2d2d2d;
  border-radius: 22px;
  padding: 1.2rem 1.2rem 1.5rem 1.2rem;
  margin: 0;
  box-shadow: 0 24px 80px rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: stretch;
  position: relative;
}
.viewer-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 0.9rem;
  flex-wrap: wrap;
  position: relative;
  min-height: 48px;
  z-index: 10;
}
.viewer-kicker {
  margin: 0 0 0.2rem;
  color: #e05c2a;
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  font-weight: 700;
}
.viewer-title {
  margin: 0;
  color: #f4f4f4;
  font-size: 1.35rem;
  font-family: 'Sora', sans-serif;
  font-weight: 700;
  letter-spacing: -0.01em;
  line-height: 1.2;
}
.viewer-close {
  width: 48px;
  height: 48px;
  border: 2px solid #e05c2a;
  border-radius: 16px;
  background: #181818;
  color: #fff;
  font-size: 2.1rem;
  font-weight: bold;
  line-height: 1;
  cursor: pointer;
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  z-index: 100;
  transition: background 0.2s, color 0.2s, border-color 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 8px rgba(0,0,0,0.18);
}
.viewer-close:hover, .viewer-close:focus-visible {
  background: #e05c2a;
  color: #fff;
  border-color: #fff;
  outline: 2px solid #fff;
}
.viewer-align-controls {
  display: flex;
  gap: 0.25rem;
  align-items: center;
  margin-left: 1.2rem;
}
.viewer-align-controls button {
  background: #191919;
  color: #e05c2a;
  border: 1px solid #2d2d2d;
  border-radius: 6px;
  width: 28px;
  height: 28px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}
.viewer-align-controls button.active,
.viewer-align-controls button:focus-visible {
  background: #e05c2a;
  color: #fff;
  outline: 2px solid #e05c2a;
}
.viewer-media {
  width: 480px;
  height: 320px;
  max-width: 90vw;
  max-height: 60vh;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1.2rem auto;
  background: #181818;
  border-radius: 14px;
  overflow: hidden;
}
.viewer-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
  transition: transform 0.4s ease;
}
.viewer-img-error {
  width: 100%;
  min-height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #222;
  color: #fff;
  border-radius: 14px;
  font-size: 1.1rem;
  margin: 0.5rem 0;
}
.viewer-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border: 1px solid rgba(255, 255, 255, 0.16);
  border-radius: 999px;
  background: rgba(16, 16, 16, 0.75);
  color: #fff;
  font-size: 1.8rem;
  line-height: 1;
  cursor: pointer;
}
.viewer-prev { left: 0.75rem; }
.viewer-next { right: 0.75rem; }
.viewer-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin: 0.9rem 0 1rem;
}
.viewer-desc {
  margin: 0;
  color: #b7b7b7;
  font-size: 0.9rem;
  line-height: 1.6;
}
.viewer-count {
  color: #b4b4b4;
  font-size: 0.8rem;
  white-space: nowrap;
}
.viewer-thumbs {
  display: flex;
  gap: 0.6rem;
  flex-wrap: wrap;
  margin-bottom: 1rem;
}
.viewer-thumb {
  width: 74px;
  height: 54px;
  padding: 0;
  border: 2px solid transparent;
  border-radius: 12px;
  overflow: hidden;
  background: #151515;
  cursor: pointer;
}
.viewer-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.viewer-thumb.active {
  border-color: #e05c2a;
}
.viewer-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}
.viewer-tag {
  font-size: 0.72rem;
  color: #efefef;
  background: #1b1b1b;
  border: 1px solid #2c2c2c;
  border-radius: 100px;
  padding: 0.2rem 0.65rem;
}


@media (max-width: 720px) {
  .project-actions {
    flex-direction: column;
    align-items: stretch;
  }
  .action-btn,
  .action-chip {
    width: 100%;
  }
  .viewer-panel {
    padding: 0.85rem;
    border-radius: 18px;
  }
  .viewer-media {
    padding-top: 0.45rem;
  }
  .viewer-img {
    max-height: 58vh;
  }
  .viewer-meta {
    flex-direction: column;
    align-items: flex-start;
  }
  .viewer-nav {
    width: 38px;
    height: 38px;
  }
  .viewer-prev { left: 0.4rem; }
  .viewer-next { right: 0.4rem; }
}
</style>
