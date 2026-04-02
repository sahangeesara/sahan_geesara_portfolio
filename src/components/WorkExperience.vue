<template>
  <section class="exp-section" id="experience">

    <!-- Header -->
    <div class="exp-header">
      <span class="exp-tag">Career</span>
      <h2 class="exp-title">Work Experience</h2>
      <div class="exp-underline"></div>
      <p class="exp-sub">Where I've grown and contributed</p>
    </div>

    <!-- Timeline -->
    <div class="exp-timeline">
      <div class="timeline-line"></div>

      <!-- Card -->
      <div class="exp-card">
        <div class="exp-dot">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="7" width="20" height="14" rx="2"/>
            <path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/>
          </svg>
        </div>

        <div class="exp-body">
          <div class="exp-top">
            <div>
              <div class="exp-badge">Internship</div>
              <h3 class="exp-role">Laravel Developer</h3>
              <p class="exp-company">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
                  <polyline points="9 22 9 12 15 12 15 22"/>
                </svg>
                Nexmatrixlab
              </p>
            </div>
            <div class="exp-duration-block">
              <span class="exp-duration-num">6</span>
              <span class="exp-duration-label">Months</span>
            </div>
          </div>

          <div class="exp-divider"></div>

          <div class="exp-meta-row">
            <span class="exp-meta-item">
              <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/>
              </svg>
              Nov 13, 2023
            </span>
            <span class="exp-arrow">→</span>
            <span class="exp-meta-item">
              <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/>
              </svg>
              May 13, 2024
            </span>
          </div>

          <div class="exp-chips">
            <span class="exp-chip">Laravel</span>
            <span class="exp-chip">PHP</span>
            <span class="exp-chip">Full Stack</span>
            <span class="exp-chip">Web Development</span>
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<script setup>
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

/* ── Section ── */
.exp-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 5rem;
  box-sizing: border-box;
}

/* ── Header ── */
.exp-header {
  text-align: center;
  margin-bottom: 4rem;
}
.exp-tag {
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
.exp-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.exp-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.exp-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* ── Timeline ── */
.exp-timeline {
  max-width: 720px;
  margin: 0 auto;
  position: relative;
}
.timeline-line {
  position: absolute;
  left: 22px;
  top: 12px;
  bottom: 12px;
  width: 1px;
  background: linear-gradient(to bottom, #e05c2a44, transparent);
}

/* ── Card row ── */
.exp-card {
  display: flex;
  gap: 1.5rem;
  align-items: flex-start;
  animation: fadeUp 0.6s ease both;
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Icon dot ── */
.exp-dot {
  flex-shrink: 0;
  width: 46px;
  height: 46px;
  background: #161616;
  border: 1px solid rgba(224, 92, 42, 0.35);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #e05c2a;
  position: relative;
  z-index: 1;
  transition: background 0.3s, border-color 0.3s;
}
.exp-dot svg { width: 20px; height: 20px; }
.exp-card:hover .exp-dot {
  background: rgba(224, 92, 42, 0.1);
  border-color: rgba(224, 92, 42, 0.6);
}

/* ── Body ── */
.exp-body {
  flex: 1;
  background: #161616;
  border: 1px solid #242424;
  border-radius: 16px;
  padding: 1.5rem 1.6rem;
  transition: border-color 0.3s, transform 0.3s;
}
.exp-card:hover .exp-body {
  border-color: rgba(224, 92, 42, 0.35);
  transform: translateX(4px);
}

/* ── Top row ── */
.exp-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1rem;
}

.exp-badge {
  display: inline-block;
  font-size: 0.68rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: #e05c2a;
  background: rgba(224, 92, 42, 0.1);
  border: 1px solid rgba(224, 92, 42, 0.2);
  border-radius: 100px;
  padding: 0.2rem 0.75rem;
  margin-bottom: 0.6rem;
}

.exp-role {
  font-family: 'Sora', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  color: #f0f0f0;
  letter-spacing: -0.01em;
  margin: 0 0 0.5rem;
}

.exp-company {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.82rem;
  color: #888;
  margin: 0;
}
.exp-company svg { color: #e05c2a; opacity: 0.7; flex-shrink: 0; }

/* ── Duration block ── */
.exp-duration-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: rgba(224, 92, 42, 0.08);
  border: 1px solid rgba(224, 92, 42, 0.2);
  border-radius: 12px;
  padding: 0.6rem 1rem;
  flex-shrink: 0;
}
.exp-duration-num {
  font-family: 'Sora', sans-serif;
  font-size: 1.6rem;
  font-weight: 700;
  color: #e05c2a;
  line-height: 1;
}
.exp-duration-label {
  font-size: 0.68rem;
  color: #888;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-top: 0.2rem;
}

/* ── Divider ── */
.exp-divider {
  height: 1px;
  background: #222;
  margin-bottom: 1rem;
}

/* ── Date row ── */
.exp-meta-row {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}
.exp-meta-item {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.8rem;
  color: #777;
}
.exp-meta-item svg { color: #e05c2a; opacity: 0.6; }
.exp-arrow {
  color: #e05c2a;
  opacity: 0.5;
  font-size: 0.85rem;
}

/* ── Chips ── */
.exp-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.exp-chip {
  font-size: 0.72rem;
  font-weight: 500;
  color: #777;
  background: #1e1e1e;
  border: 1px solid #2a2a2a;
  border-radius: 6px;
  padding: 0.2rem 0.65rem;
  transition: color 0.2s, border-color 0.2s;
}
.exp-chip:hover {
  color: #e05c2a;
  border-color: rgba(224, 92, 42, 0.35);
}

/* ── Responsive ── */
.exp-top { flex-wrap: nowrap; }
@media (max-width: 560px) {
  .timeline-line { display: none; }
  .exp-card { flex-direction: column; gap: 0.75rem; }
  .exp-dot  { width: 40px; height: 40px; border-radius: 10px; }
  .exp-top  { flex-wrap: wrap; }
}
</style>