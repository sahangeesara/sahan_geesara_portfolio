<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { RouterView } from 'vue-router'

const menuOpen   = ref(false)
const activeLink = ref('home')
const scrolled   = ref(false)

const sections = ['home','about','project','skills','education','certificate','experience','contact']

function toggleMenu() { menuOpen.value = !menuOpen.value }
function closeMenu()  { menuOpen.value = false }

function onScroll() {
  scrolled.value = window.scrollY > 40

  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i])
    if (el && window.scrollY >= el.offsetTop - 100) {
      activeLink.value = sections[i]
      break
    }
  }
}

onMounted(()  => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<template>
  <!-- ── Navbar ── -->
  <nav class="nav-bar" :class="{ scrolled }">
    <div class="nav-container">

      <!-- Brand -->
      <a class="nav-brand" href="#home" @click="closeMenu">Sahan</a>

      <!-- Desktop links -->
      <ul class="nav-links">
        <li v-for="s in sections" :key="s">
          <a
            :href="`#${s}`"
            class="nav-link"
            :class="{ active: activeLink === s }"
          >{{ s === 'certificate' ? 'Certificates' : s === 'experience' ? 'Work Experience' : s.charAt(0).toUpperCase() + s.slice(1) }}</a>
        </li>
      </ul>

      <!-- Hamburger -->
      <button class="nav-toggle" :class="{ open: menuOpen }" @click="toggleMenu" aria-label="Toggle menu">
        <span></span><span></span><span></span>
      </button>
    </div>

    <!-- Mobile drawer -->
    <div class="nav-drawer" :class="{ open: menuOpen }">
      <ul class="drawer-links">
        <li v-for="s in sections" :key="s">
          <a
            :href="`#${s}`"
            class="drawer-link"
            :class="{ active: activeLink === s }"
            @click="closeMenu"
          >{{ s === 'certificate' ? 'Certificates' : s === 'experience' ? 'Work Experience' : s.charAt(0).toUpperCase() + s.slice(1) }}</a>
        </li>
      </ul>
    </div>
  </nav>

  <!-- ── Page content ── -->
  <div class="homepage">
    <RouterView />
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

/* ── Navbar shell ── */
.nav-bar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 999;
  background: #0d0d0d;
  border-bottom: 1px solid #1a1a1a;
  transition: border-color 0.3s, box-shadow 0.3s;
  font-family: 'DM Sans', sans-serif;
}
.nav-bar.scrolled {
  border-bottom-color: rgba(224, 92, 42, 0.25);
  box-shadow: 0 4px 32px rgba(0,0,0,0.6);
}

.nav-container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 1.5rem;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* ── Brand ── */
.nav-brand {
  font-family: 'Sora', sans-serif;
  font-size: 1.35rem;
  font-weight: 700;
  color: #f8f8f8;
  text-decoration: none;
  letter-spacing: -0.02em;
  transition: color 0.2s;
}
.nav-brand:hover { color: #e05c2a; }

/* ── Desktop links ── */
.nav-links {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  list-style: none;
}

.nav-link {
  display: inline-block;
  padding: 0.45rem 0.85rem;
  font-size: 0.84rem;
  font-weight: 500;
  color: #888;
  text-decoration: none;
  border-radius: 8px;
  transition: color 0.2s, background 0.2s;
  white-space: nowrap;
}
.nav-link:hover {
  color: #f0f0f0;
  background: rgba(255,255,255,0.05);
}
.nav-link.active {
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.1);
}

/* ── Hamburger ── */
.nav-toggle {
  display: none;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  width: 36px;
  height: 36px;
  background: #161616;
  border: 1px solid #2a2a2a;
  border-radius: 8px;
  cursor: pointer;
  padding: 8px;
  transition: border-color 0.2s;
}
.nav-toggle:hover { border-color: rgba(224,92,42,0.5); }

.nav-toggle span {
  display: block;
  height: 2px;
  background: #ccc;
  border-radius: 2px;
  transition: transform 0.3s, opacity 0.3s, background 0.2s;
  transform-origin: center;
}
.nav-toggle.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); background: #e05c2a; }
.nav-toggle.open span:nth-child(2) { opacity: 0; }
.nav-toggle.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); background: #e05c2a; }

/* ── Mobile drawer ── */
.nav-drawer {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.35s ease, border-top-color 0.3s;
  border-top: 1px solid transparent;
  background: #0d0d0d;
}
.nav-drawer.open {
  max-height: 420px;
  border-top-color: #1e1e1e;
}

.drawer-links {
  list-style: none;
  padding: 0.75rem 1.5rem 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}
.drawer-link {
  display: block;
  padding: 0.65rem 1rem;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  color: #888;
  text-decoration: none;
  border-radius: 10px;
  transition: color 0.2s, background 0.2s, padding-left 0.2s;
}
.drawer-link:hover {
  color: #f0f0f0;
  background: rgba(255,255,255,0.05);
  padding-left: 1.35rem;
}
.drawer-link.active {
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.1);
}

/* ── Responsive ── */
@media (max-width: 860px) {
  .nav-links  { display: none; }
  .nav-toggle { display: flex; }
}

/* ── Page ── */
.homepage {
  margin-top: 64px;
  background-color: #0d0d0d;
}
</style>