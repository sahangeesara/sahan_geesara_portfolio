<template>
  <section class="edu-section" id="education">

    <!-- Header -->
    <div class="edu-header">
      <span class="edu-tag">Academic Background</span>
      <h2 class="edu-title">Education</h2>
      <div class="edu-underline"></div>
      <p class="edu-sub">The foundations that shaped my skills</p>
    </div>

    <!-- Timeline -->
    <div class="edu-timeline">

      <!-- Line -->
      <div class="timeline-line"></div>

      <!-- Card 1 -->
      <div class="edu-card" style="animation-delay: 0.1s">
        <div class="edu-dot">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
            <path d="M6 12v5c3 3 9 3 12 0v-5"/>
          </svg>
        </div>
        <div class="edu-body">
          <div class="edu-badge">Undergraduate</div>
          <h3 class="edu-degree">Bachelor of Information Technology</h3>
          <p class="edu-inst">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
            </svg>
            University of Colombo School of Computing (UCSC)
          </p>
          <p class="edu-desc">
            The degree program's primary focus was the development of Computer-Based Information Systems for businesses.
            Completed the first year with a GPA of 2.38.
          </p>
          <div class="edu-meta">
            <span class="meta-chip">GPA 2.38</span>
            <span class="meta-chip">CBIS Focus</span>
          </div>
        </div>
      </div>

      <!-- Card 2 -->
      <div class="edu-card" style="animation-delay: 0.25s">
        <div class="edu-dot">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/>
            <line x1="12" y1="12" x2="12" y2="16"/><line x1="10" y1="14" x2="14" y2="14"/>
          </svg>
        </div>
        <div class="edu-body">
          <div class="edu-badge">Diploma</div>
          <h3 class="edu-degree">Software Engineering Diploma</h3>
          <p class="edu-inst">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
            </svg>
            ICET Institute
          </p>
          <p class="edu-desc">
            Completed a comprehensive diploma in Software Engineering at ICET Institute, building core competencies
            in software development principles and practices.
          </p>
          <div class="edu-meta">
            <span class="meta-chip">Software Engineering</span>
            <span class="meta-chip">ICET</span>
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
.edu-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 5rem;
  box-sizing: border-box;
}

/* ── Header ── */
.edu-header {
  text-align: center;
  margin-bottom: 4rem;
}
.edu-tag {
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
.edu-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.edu-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.edu-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* ── Timeline wrapper ── */
.edu-timeline {
  max-width: 720px;
  margin: 0 auto;
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.timeline-line {
  position: absolute;
  left: 22px;
  top: 12px;
  bottom: 12px;
  width: 1px;
  background: linear-gradient(to bottom, #e05c2a44, #f5a62322, transparent);
}

/* ── Card ── */
.edu-card {
  display: flex;
  gap: 1.5rem;
  align-items: flex-start;
  animation: fadeUp 0.6s ease both;
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Dot icon ── */
.edu-dot {
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
.edu-dot svg { width: 20px; height: 20px; }
.edu-card:hover .edu-dot {
  background: rgba(224, 92, 42, 0.1);
  border-color: rgba(224, 92, 42, 0.6);
}

/* ── Body ── */
.edu-body {
  flex: 1;
  background: #161616;
  border: 1px solid #242424;
  border-radius: 16px;
  padding: 1.5rem 1.6rem;
  transition: border-color 0.3s, transform 0.3s;
}
.edu-card:hover .edu-body {
  border-color: rgba(224, 92, 42, 0.35);
  transform: translateX(4px);
}

.edu-badge {
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
  margin-bottom: 0.75rem;
}

.edu-degree {
  font-family: 'Sora', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  color: #f0f0f0;
  letter-spacing: -0.01em;
  margin: 0 0 0.6rem;
}

.edu-inst {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.82rem;
  color: #888;
  margin: 0 0 0.75rem;
}
.edu-inst svg { flex-shrink: 0; color: #e05c2a; opacity: 0.7; }

.edu-desc {
  font-size: 0.87rem;
  color: #666;
  line-height: 1.7;
  margin: 0 0 1rem;
}

/* ── Meta chips ── */
.edu-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.meta-chip {
  font-size: 0.72rem;
  font-weight: 500;
  color: #777;
  background: #1e1e1e;
  border: 1px solid #2a2a2a;
  border-radius: 6px;
  padding: 0.2rem 0.65rem;
}

/* ── Responsive ── */
@media (max-width: 560px) {
  .timeline-line { display: none; }
  .edu-card { flex-direction: column; gap: 0.75rem; }
  .edu-dot  { width: 40px; height: 40px; border-radius: 10px; }
}
</style>