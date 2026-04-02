<template>
  <section class="hero-section" id="home">
    <div class="hero-inner">

      <!-- Left: Text -->
      <div class="hero-text">
        <span class="hero-tag">Available for work</span>

        <h1 class="hero-name">Sahan<br />Geesara</h1>

        <div class="hero-underline"></div>

        <h5 class="hero-role">
          <span class="role-cursor">&lt;</span>
          FullStack Developer
          <span class="role-cursor">/&gt;</span>
        </h5>

        <p class="hero-bio">
          Hi! I'm a creative developer based in Sri Lanka —
          passionate, dedicated, and always building something new.
        </p>

        <ul class="hero-traits">
          <li>
            <span class="trait-dot"></span>
            Quick Learner
          </li>
          <li>
            <span class="trait-dot"></span>
            Team Player
          </li>
          <li>
            <span class="trait-dot"></span>
            Hard Worker
          </li>
        </ul>

        <div class="hero-socials">
          <a href="https://www.instagram.com/sahan_g_samarawicrama_?igsh=czNwaHV4ZmhqZ2s5"
             target="_blank" aria-label="Instagram" class="social-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
              <circle cx="12" cy="12" r="4"/>
              <circle cx="17.5" cy="6.5" r="0.5" fill="currentColor" stroke="none"/>
            </svg>
          </a>
          <a href="https://www.linkedin.com/in/sahan-g-samaravickrama-83b5001aa/"
             target="_blank" aria-label="LinkedIn" class="social-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
              <rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/>
            </svg>
          </a>
          <a href="https://github.com/sahan99827"
             target="_blank" aria-label="GitHub" class="social-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/>
            </svg>
          </a>
        </div>
      </div>

      <!-- Right: Image -->
      <div class="hero-img-wrap">
        <div class="hero-img-ring"></div>
        <img src="/assets/1.png" alt="Sahan Geesara" class="hero-img" />
        <div class="hero-img-glow"></div>
      </div>

    </div>

    <!-- Background decoration -->
    <div class="hero-bg-orb orb1"></div>
    <div class="hero-bg-orb orb2"></div>
  </section>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

/* ── Section ── */
.hero-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 6rem 1.5rem 4rem;
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
}

/* ── Background orbs ── */
.hero-bg-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
  z-index: 0;
}
.orb1 {
  width: 420px; height: 420px;
  background: rgba(224, 92, 42, 0.08);
  top: -80px; left: -100px;
}
.orb2 {
  width: 300px; height: 300px;
  background: rgba(245, 166, 35, 0.06);
  bottom: 0; right: -60px;
}

/* ── Inner layout ── */
.hero-inner {
  max-width: 1100px;
  margin: 0 auto;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 3rem;
  position: relative;
  z-index: 1;
}

/* ── Text side ── */
.hero-text {
  flex: 1;
  animation: fadeSlide 0.8s ease both;
}

.hero-tag {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.1);
  border: 1px solid rgba(224, 92, 42, 0.3);
  border-radius: 100px;
  padding: 0.35rem 1rem;
  margin-bottom: 1.4rem;
}
.hero-tag::before {
  content: '';
  width: 7px; height: 7px;
  background: #e05c2a;
  border-radius: 50%;
  animation: pulse 1.8s ease-in-out infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%       { opacity: 0.4; transform: scale(0.7); }
}

.hero-name {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2.8rem, 7vw, 5rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.03em;
  line-height: 1.05;
  margin: 0 0 1rem;
  animation: fadeSlide 0.9s ease both;
}

.hero-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin-bottom: 1.2rem;
  animation: fadeSlide 1s ease both;
}

.hero-role {
  font-family: 'Sora', sans-serif;
  font-size: 1.05rem;
  font-weight: 600;
  color: #888;
  margin-bottom: 1.2rem;
  animation: fadeSlide 1.05s ease both;
}
.role-cursor {
  color: #e05c2a;
  opacity: 0.7;
}

.hero-bio {
  font-size: 0.95rem;
  color: #666;
  line-height: 1.7;
  max-width: 420px;
  margin-bottom: 1.5rem;
  animation: fadeSlide 1.1s ease both;
}

/* ── Traits ── */
.hero-traits {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  margin-bottom: 2rem;
  animation: fadeSlide 1.15s ease both;
}
.hero-traits li {
  display: flex;
  align-items: center;
  gap: 0.65rem;
  font-size: 0.88rem;
  color: #aaa;
  font-weight: 500;
}
.trait-dot {
  width: 7px; height: 7px;
  background: linear-gradient(135deg, #e05c2a, #f5a623);
  border-radius: 50%;
  flex-shrink: 0;
}

/* ── Socials ── */
.hero-socials {
  display: flex;
  gap: 0.75rem;
  animation: fadeSlide 1.2s ease both;
}
.social-btn {
  width: 44px; height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #161616;
  border: 1px solid #2a2a2a;
  border-radius: 12px;
  color: #888;
  text-decoration: none;
  transition: color 0.25s, border-color 0.25s, background 0.25s, transform 0.25s;
}
.social-btn svg { width: 18px; height: 18px; }
.social-btn:hover {
  color: #e05c2a;
  border-color: rgba(224, 92, 42, 0.5);
  background: rgba(224, 92, 42, 0.08);
  transform: translateY(-3px);
}

/* ── Image side ── */
.hero-img-wrap {
  position: relative;
  flex-shrink: 0;
  animation: zoomFloat 1.4s ease both;
}

.hero-img {
  width: 300px;
  height: 380px;
  object-fit: cover;
  border-radius: 140px 140px 120px 120px;
  border: 3px solid rgba(224, 92, 42, 0.35);
  display: block;
  position: relative;
  z-index: 2;
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}
.hero-img:hover {
  transform: scale(1.04) translateY(-6px);
  box-shadow: 0 30px 60px rgba(224, 92, 42, 0.18);
}

.hero-img-ring {
  position: absolute;
  inset: -14px;
  border-radius: 160px 160px 140px 140px;
  border: 1px solid rgba(224, 92, 42, 0.15);
  z-index: 1;
  animation: spinRing 12s linear infinite;
}
@keyframes spinRing {
  to { transform: rotate(360deg); }
}

.hero-img-glow {
  position: absolute;
  inset: 0;
  border-radius: 140px 140px 120px 120px;
  background: radial-gradient(ellipse at center, rgba(224,92,42,0.12) 0%, transparent 70%);
  z-index: 1;
  pointer-events: none;
}

/* ── Animations ── */
@keyframes fadeSlide {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes zoomFloat {
  0%   { opacity: 0; transform: scale(0.88) translateY(20px); }
  60%  { opacity: 1; transform: scale(1) translateY(0); }
  100% { transform: scale(1) translateY(0); }
}

/* ── Responsive ── */
@media (max-width: 820px) {
  .hero-inner {
    flex-direction: column-reverse;
    text-align: center;
    gap: 2.5rem;
  }
  .hero-bio   { margin: 0 auto 1.5rem; }
  .hero-traits li { justify-content: center; }
  .hero-socials   { justify-content: center; }
  .hero-underline { margin: 0 auto 1.2rem; }
}

@media (max-width: 480px) {
  .hero-img { width: 240px; height: 300px; }
  .hero-section { padding: 5rem 1rem 3rem; }
}
</style>

<script setup>
</script>