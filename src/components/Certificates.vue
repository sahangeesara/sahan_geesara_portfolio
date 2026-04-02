<template>
  <section class="cert-section">

    <!-- Header -->
    <div class="cert-header">
      <span class="cert-tag">My Achievements</span>
      <h1 class="cert-title">Certificates</h1>
      <div class="cert-underline"></div>
      <p class="cert-sub">{{ certificates.length }} certifications earned</p>
    </div>

    <!-- Grid -->
    <div class="cert-grid">
      <div
        v-for="(cert, i) in paginatedCerts"
        :key="cert.src"
        class="cert-card"
        :style="{ animationDelay: `${i * 0.07}s` }"
        @click="openLightbox(cert)"
      >
        <div class="cert-img-wrap">
          <img :src="cert.src" :alt="cert.alt" class="cert-img" loading="lazy" />
          <div class="cert-overlay">
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/>
              <line x1="11" y1="8" x2="11" y2="14"/><line x1="8" y1="11" x2="14" y2="11"/>
            </svg>
            <span>View</span>
          </div>
        </div>
        <div class="cert-footer">
          <span class="cert-num">{{ String(startIndex + i + 1).padStart(2, '0') }}</span>
          <span class="cert-label">{{ cert.alt }}</span>
        </div>
      </div>
    </div>

    <!-- Pagination -->
    <div class="cert-pagination">
      <button class="pg-btn" :disabled="currentPage === 1" @click="prevPage">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
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
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="9 18 15 12 9 6"/>
        </svg>
      </button>
    </div>

    <p class="pg-info">Page {{ currentPage }} of {{ totalPages }}</p>

    <!-- Lightbox -->
    <Transition name="lb">
      <div v-if="lightbox" class="lightbox" @click.self="closeLightbox">
        <div class="lb-box">
          <button class="lb-close" @click="closeLightbox">
            <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
            </svg>
          </button>
          <button class="lb-nav lb-prev" @click="lightboxPrev">
            <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="15 18 9 12 15 6"/>
            </svg>
          </button>
          <img :src="lightbox.src" :alt="lightbox.alt" class="lb-img" />
          <button class="lb-nav lb-next" @click="lightboxNext">
            <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="9 18 15 12 9 6"/>
            </svg>
          </button>
          <div class="lb-caption">{{ lightbox.alt }} &mdash; {{ lightboxIndex + 1 }} / {{ certificates.length }}</div>
        </div>
      </div>
    </Transition>

  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const ITEMS_PER_PAGE = 6

const certificates = [
  { src: '/assets/Certificates/1.jpeg',  alt: 'Certificate 1'  },
  { src: '/assets/Certificates/2.jpeg',  alt: 'Certificate 2'  },
  { src: '/assets/Certificates/3.jpeg',  alt: 'Certificate 3'  },
  { src: '/assets/Certificates/4.jpeg',  alt: 'Certificate 4'  },
  { src: '/assets/Certificates/5.jpeg',  alt: 'Certificate 5'  },
  { src: '/assets/Certificates/6.jpg',   alt: 'Certificate 6'  },
  { src: '/assets/Certificates/7.png',   alt: 'Certificate 7'  },
  { src: '/assets/Certificates/8.png',   alt: 'Certificate 8'  },
  { src: '/assets/Certificates/9.png',   alt: 'Certificate 9'  },
  { src: '/assets/Certificates/10.png',  alt: 'Certificate 10' },
  { src: '/assets/Certificates/11.png',  alt: 'Certificate 11' },
  { src: '/assets/Certificates/12.png',  alt: 'Certificate 12' },
  { src: '/assets/Certificates/13.png',  alt: 'Certificate 13' },
  { src: '/assets/Certificates/14.png',  alt: 'Certificate 14' },
  { src: '/assets/Certificates/15.png',  alt: 'Certificate 15' },
  { src: '/assets/Certificates/16.png',  alt: 'Certificate 16' },
  { src: '/assets/Certificates/17.png',  alt: 'Certificate 17' },
]

const currentPage  = ref(1)
const lightbox     = ref(null)
const lightboxIndex = ref(0)

const totalPages = computed(() => Math.ceil(certificates.length / ITEMS_PER_PAGE))
const startIndex = computed(() => (currentPage.value - 1) * ITEMS_PER_PAGE)
const paginatedCerts = computed(() =>
  certificates.slice(startIndex.value, startIndex.value + ITEMS_PER_PAGE)
)

function prevPage() { if (currentPage.value > 1) currentPage.value-- }
function nextPage() { if (currentPage.value < totalPages.value) currentPage.value++ }
function goToPage(p) { currentPage.value = p }

function openLightbox(cert) {
  lightboxIndex.value = certificates.indexOf(cert)
  lightbox.value = cert
}
function closeLightbox() { lightbox.value = null }
function lightboxPrev() {
  lightboxIndex.value = (lightboxIndex.value - 1 + certificates.length) % certificates.length
  lightbox.value = certificates[lightboxIndex.value]
}
function lightboxNext() {
  lightboxIndex.value = (lightboxIndex.value + 1) % certificates.length
  lightbox.value = certificates[lightboxIndex.value]
}

function onKey(e) {
  if (!lightbox.value) return
  if (e.key === 'Escape')     closeLightbox()
  if (e.key === 'ArrowLeft')  lightboxPrev()
  if (e.key === 'ArrowRight') lightboxNext()
}
onMounted(()  => window.addEventListener('keydown', onKey))
onUnmounted(() => window.removeEventListener('keydown', onKey))
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

/* ── Section ── */
.cert-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 4rem;
  min-height: 100vh;
  box-sizing: border-box;
}

/* ── Header ── */
.cert-header {
  text-align: center;
  margin-bottom: 3.5rem;
}
.cert-tag {
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
.cert-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.cert-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.cert-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* ── Grid ── */
.cert-grid {
  max-width: 1100px;
  margin: 0 auto 2.5rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.4rem;
}
@media (max-width: 900px) {
  .cert-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 560px) {
  .cert-grid { grid-template-columns: 1fr; }
}

/* ── Card ── */
.cert-card {
  background: #161616;
  border: 1px solid #242424;
  border-radius: 16px;
  overflow: hidden;
  cursor: pointer;
  animation: fadeUp 0.5s ease both;
  transition: border-color 0.3s, transform 0.3s;
}
.cert-card:hover {
  border-color: rgba(224, 92, 42, 0.5);
  transform: translateY(-4px);
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.cert-img-wrap {
  position: relative;
  width: 100%;
  aspect-ratio: 4 / 3;
  overflow: hidden;
}
.cert-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.4s ease;
}
.cert-card:hover .cert-img {
  transform: scale(1.06);
}

.cert-overlay {
  position: absolute;
  inset: 0;
  background: rgba(224, 92, 42, 0.75);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.4rem;
  color: #fff;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.06em;
  opacity: 0;
  transition: opacity 0.3s ease;
}
.cert-card:hover .cert-overlay {
  opacity: 1;
}

.cert-footer {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  border-top: 1px solid #222;
}
.cert-num {
  font-family: 'Sora', sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  color: #e05c2a;
  opacity: 0.7;
}
.cert-label {
  font-size: 0.8rem;
  color: #777;
  font-weight: 500;
}

/* ── Pagination ── */
.cert-pagination {
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
.pg-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
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

.pg-info {
  text-align: center;
  font-size: 0.78rem;
  color: #555;
  margin: 0;
}

/* ── Lightbox ── */
.lightbox {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.92);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}
.lb-box {
  position: relative;
  max-width: 860px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}
.lb-img {
  max-width: 100%;
  max-height: 75vh;
  object-fit: contain;
  border-radius: 12px;
  border: 1px solid #333;
  display: block;
}
.lb-close {
  position: absolute;
  top: -3rem;
  right: 0;
  background: #222;
  border: 1px solid #333;
  color: #ccc;
  border-radius: 8px;
  padding: 0.4rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: color 0.2s, border-color 0.2s;
}
.lb-close:hover { color: #e05c2a; border-color: #e05c2a; }

.lb-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(30,30,30,0.85);
  border: 1px solid #333;
  color: #ccc;
  border-radius: 50%;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: color 0.2s, border-color 0.2s, background 0.2s;
}
.lb-nav:hover { color: #e05c2a; border-color: #e05c2a; background: rgba(224,92,42,0.1); }
.lb-prev { left: -56px; }
.lb-next { right: -56px; }

@media (max-width: 600px) {
  .lb-prev { left: 0; }
  .lb-next { right: 0; }
  .lb-img  { max-height: 65vh; }
}

.lb-caption {
  font-size: 0.82rem;
  color: #666;
  text-align: center;
}

/* ── Transitions ── */
.lb-enter-active, .lb-leave-active { transition: opacity 0.25s ease; }
.lb-enter-from, .lb-leave-to { opacity: 0; }
</style>