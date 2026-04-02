<template>
  <section class="contact-section">

    <!-- Header -->
    <div class="contact-header">
      <span class="contact-tag">Reach Out</span>
      <h1 class="contact-title">Get in <span class="accent">Touch</span></h1>
      <div class="contact-underline"></div>
      <p class="contact-sub">I'm always open to new opportunities and conversations</p>
    </div>

    <div class="contact-layout">

      <!-- Left: Contact Cards -->
      <div class="contact-cards">
        <a
          v-for="card in contactCards"
          :key="card.label"
          :href="card.href"
          :target="card.external ? '_blank' : '_self'"
          class="contact-card"
          rel="noopener noreferrer"
        >
          <div class="cc-icon" v-html="card.icon"></div>
          <div class="cc-body">
            <h4 class="cc-label">{{ card.label }}</h4>
            <p class="cc-value">{{ card.value }}</p>
            <span class="cc-action">
              Write me
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/>
              </svg>
            </span>
          </div>
        </a>
      </div>

      <!-- Right: Form -->
      <form class="contact-form" ref="form" @submit.prevent="sendEmail" id="emailForm" novalidate>

        <div class="form-group">
          <label class="form-label">Name</label>
          <input
            type="text"
            class="form-input"
            name="user_name"
            v-model="formData.name"
            placeholder="Your full name"
            autocomplete="name"
          />
        </div>

        <div class="form-group">
          <label class="form-label">Email</label>
          <input
            type="email"
            class="form-input"
            name="form_email"
            v-model="formData.email"
            placeholder="your@email.com"
            autocomplete="email"
          />
        </div>

        <div class="form-group">
          <label class="form-label">Message</label>
          <textarea
            class="form-input form-textarea"
            name="message"
            v-model="formData.message"
            placeholder="Tell me about your project or just say hello..."
            rows="6"
          ></textarea>
        </div>

        <!-- Status -->
        <Transition name="fade">
          <div v-if="status" class="form-status" :class="status.type">
            <span v-html="status.icon"></span>
            {{ status.message }}
          </div>
        </Transition>

        <button
          type="submit"
          class="form-btn"
          :disabled="!isFormValid || loading"
        >
          <span v-if="loading" class="btn-spinner"></span>
          <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/>
          </svg>
          {{ loading ? 'Sending…' : 'Send Message' }}
        </button>

      </form>
    </div>

  </section>
</template>

<script>
import emailjs from '@emailjs/browser'

export default {
  data() {
    return {
      loading: false,
      status: null,
      formData: { name: '', email: '', message: '' }
    }
  },
  computed: {
    isFormValid() {
      return this.formData.name.trim() && this.formData.email.trim() && this.formData.message.trim()
    },
    contactCards() {
      return [
        {
          label: 'Email',
          value: 'geesarasahan0123@gmail.com',
          href: '#emailForm',
          external: false,
          icon: `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`
        },
        {
          label: 'WhatsApp',
          value: '+94 78 8392 166',
          href: 'https://wa.me/qr/H7BXGMNNP5Z3A1',
          external: true,
          icon: `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/></svg>`
        },
        {
          label: 'Facebook',
          value: 'sahan-g-samaravicrama',
          href: 'https://www.facebook.com/@sahan.gsamaravicrama/',
          external: true,
          icon: `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/></svg>`
        }
      ]
    }
  },
  methods: {
    sendEmail() {
      this.loading = true
      this.status = null

      emailjs.sendForm('service_jfkxjtz', 'template_1uxngrt', this.$refs.form, {
        publicKey: 'E3o89W-UvbW1YLFjg'
      })
      .then(() => {
        this.status = {
          type: 'success',
          message: 'Message sent successfully! I\'ll get back to you soon.',
          icon: `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>`
        }
        this.$refs.form.reset()
        this.formData = { name: '', email: '', message: '' }
        this.loading = false
        setTimeout(() => { this.status = null }, 5000)
      })
      .catch((error) => {
        console.error('EmailJS error:', error)
        this.status = {
          type: 'error',
          message: 'Something went wrong. Please try again.',
          icon: `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>`
        }
        this.loading = false
      })
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&family=DM+Sans:wght@400;500&display=swap');

/* ── Section ── */
.contact-section {
  font-family: 'DM Sans', sans-serif;
  background: #0d0d0d;
  color: #f0f0f0;
  padding: 6rem 1.5rem 5rem;
  min-height: 100vh;
  box-sizing: border-box;
}

/* ── Header ── */
.contact-header {
  text-align: center;
  margin-bottom: 3.5rem;
}
.contact-tag {
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
.contact-title {
  font-family: 'Sora', sans-serif;
  font-size: clamp(2rem, 5vw, 3.2rem);
  font-weight: 700;
  color: #f8f8f8;
  letter-spacing: -0.02em;
  margin: 0.4rem 0 0.8rem;
}
.accent { color: #e05c2a; }
.contact-underline {
  width: 56px;
  height: 4px;
  background: linear-gradient(90deg, #e05c2a, #f5a623);
  border-radius: 2px;
  margin: 0 auto 1rem;
}
.contact-sub {
  color: #666;
  font-size: 0.88rem;
  margin: 0;
}

/* ── Layout ── */
.contact-layout {
  max-width: 1080px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1.4fr;
  gap: 3rem;
  align-items: start;
}
@media (max-width: 768px) {
  .contact-layout {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
}

/* ── Contact Cards ── */
.contact-cards {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.contact-card {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  background: #161616;
  border: 1px solid #242424;
  border-radius: 16px;
  padding: 1.25rem 1.2rem;
  text-decoration: none;
  transition: border-color 0.3s, transform 0.3s;
}
.contact-card:hover {
  border-color: rgba(224, 92, 42, 0.5);
  transform: translateX(4px);
}

.cc-icon {
  color: #e05c2a;
  line-height: 0;
  flex-shrink: 0;
  margin-top: 2px;
}

.cc-body { flex: 1; }

.cc-label {
  font-family: 'Sora', sans-serif;
  font-size: 0.95rem;
  font-weight: 700;
  color: #f0f0f0;
  margin: 0 0 0.25rem;
}

.cc-value {
  font-size: 0.8rem;
  color: #666;
  margin: 0 0 0.5rem;
  word-break: break-all;
}

.cc-action {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.8rem;
  color: #e05c2a;
  font-weight: 500;
  transition: gap 0.2s;
}
.contact-card:hover .cc-action { gap: 0.5rem; }

/* ── Form ── */
.contact-form {
  background: #161616;
  border: 1px solid #242424;
  border-radius: 18px;
  padding: 2rem 1.75rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.45rem;
}

.form-label {
  font-size: 0.8rem;
  font-weight: 600;
  color: #888;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.form-input {
  background: #111;
  border: 1px solid #2a2a2a;
  border-radius: 10px;
  color: #f0f0f0;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  padding: 0.8rem 1rem;
  outline: none;
  transition: border-color 0.2s, background 0.2s;
  width: 100%;
  box-sizing: border-box;
}
.form-input::placeholder { color: #444; }
.form-input:focus {
  border-color: rgba(224, 92, 42, 0.55);
  background: #131313;
}

.form-textarea {
  resize: vertical;
  min-height: 140px;
  line-height: 1.6;
}

/* ── Status ── */
.form-status {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.84rem;
  font-weight: 500;
  padding: 0.75rem 1rem;
  border-radius: 10px;
}
.form-status.success {
  background: rgba(16, 185, 129, 0.1);
  border: 1px solid rgba(16, 185, 129, 0.25);
  color: #34d399;
}
.form-status.error {
  background: rgba(224, 92, 42, 0.1);
  border: 1px solid rgba(224, 92, 42, 0.25);
  color: #f87171;
}

/* ── Submit Button ── */
.form-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: #e05c2a;
  color: #fff;
  border: none;
  border-radius: 12px;
  padding: 0.9rem 1.8rem;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.25s, transform 0.2s, box-shadow 0.25s;
  box-shadow: 0 4px 16px rgba(224, 92, 42, 0.35);
  width: 100%;
}
.form-btn:hover:not(:disabled) {
  background: #c94f22;
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(224, 92, 42, 0.45);
}
.form-btn:active:not(:disabled) { transform: translateY(0); }
.form-btn:disabled {
  opacity: 0.45;
  cursor: not-allowed;
  box-shadow: none;
}

/* ── Spinner ── */
.btn-spinner {
  width: 16px;
  height: 16px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
  flex-shrink: 0;
}
@keyframes spin {
  to { transform: rotate(360deg); }
}

/* ── Transitions ── */
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>