<script setup>

</script>
<template>
    <section class="hero">
  <div class="container hero-inner">
    <div class="hero-text">
      <h1>Manage Your Tickets Effortlessly</h1>
      <p>
        TicketApp helps you create, track, and resolve tickets easily — whether
        you’re managing tasks, customers, or project issues.
      </p>
      <div class="cta-buttons">
        <a href="/login" class="btn primary">Login</a>
        <a href="/signup" class="btn secondary">Get Started</a>
      </div>
    </div>

    <div class="hero-visual">
      <img src="../assets/wave.svg" alt="Decorative Wave" class="hero-wave" />
      <div class="circle-decor"></div>
    </div>
  </div>
</section>

<section class="features">
  <div class="container features-grid">
    <div class="feature-card">
      <h3>Create Tickets</h3>
      <p>Submit new tickets instantly with validation and automatic tracking.</p>
    </div>
    <div class="feature-card">
      <h3>View & Manage</h3>
      <p>See open, in-progress, and closed tickets all in one organized dashboard.</p>
    </div>
    <div class="feature-card">
      <h3>Secure & Simple</h3>
      <p>All your ticket data is safely stored in local sessions — quick and secure.</p>
    </div>
  </div>
</section>
</template>
<style scoped>
/* ===== HERO SECTION ===== */
.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 1rem 2rem;
}
.hero {
  position: relative;
  overflow: hidden;
  background: linear-gradient(135deg, #2563eb 0%, #1e3a8a 100%);
  color: white;
  padding: 6rem 0 4rem;
}

.hero-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 2rem;
  position: relative;
  z-index: 2;
}

.hero-text {
  flex: 1 1 500px;
  max-width: 600px;
}

.hero-text h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  line-height: 1.2;
}

.hero-text p {
  font-size: 1.125rem;
  margin-bottom: 2rem;
  color: #e0e7ff;
  max-width: 480px;
}

.cta-buttons {
  display: flex;
  gap: 1rem;
}

.btn {
  display: inline-block;
  padding: 0.8rem 1.5rem;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  transition: all 0.3s ease;
}

.btn.primary {
  background-color: white;
  color: #1e3a8a;
}

.btn.primary:hover {
  background-color: #e0e7ff;
}

.btn.secondary {
  background-color: transparent;
  border: 2px solid white;
  color: white;
}

.btn.secondary:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

/* ===== Wave and Circle ===== */
.hero-visual {
  position: relative;
  flex: 1 1 400px;
  min-height: 250px;
  width: 100%;
}

.hero-wave {
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 100%;
}

.circle-decor {
  position: absolute;
  top: -60px;
  right: -60px;
  width: 120px;
  height: 120px;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 50%;
  z-index: 1;
}

/* ===== FEATURES SECTION ===== */
.features {
  background: #f9fafb;
  padding: 5rem 0;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.feature-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.05);
  padding: 2rem;
  transition: transform 0.3s ease;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-card h3 {
  margin-bottom: 0.75rem;
  color: #1e3a8a;
}

.feature-card p {
  color: #4b5563;
  font-size: 1rem;
  line-height: 1.6;
}

/* ===== Responsive Hero ===== */
@media (max-width: 768px) {
  .hero-inner {
    flex-direction: column;
    text-align: center;
  }

  .hero-text h1 {
    font-size: 2rem;
  }

  .hero-text p {
    margin: 0 auto 1.5rem;
  }

  .cta-buttons {
    justify-content: center;
  }

  .circle-decor {
    top: -40px;
    right: 50%;
    transform: translateX(50%);
  }
}
</style>