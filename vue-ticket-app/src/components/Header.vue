<script setup>
import { ref } from 'vue';

  const isOpen = ref(false);

  const toggleNav = () => {
  isOpen.value = !isOpen.value;
};
</script>

<template>
    <header class="site-header">
      <div class="container nav-container">
        <h1 class="logo">🎟️ TicketApp</h1>
        <nav :class="{ open: isOpen }" class="nav-links" id="mainNav">
          <a href="/">Home</a>
          <a href="/dashboard">Dashboard</a>
          <a href="/tickets">Tickets</a>
          <a href="/login">Login</a>
        </nav>
        <button class="nav-toggle" @click="toggleNav" aria-label="Toggle navigation">☰</button>
      </div>
    </header>
</template>
<style scoped>

/* ===== Layout Containers ===== */
.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 1rem 2rem;
}

/* ===== Header ===== */
.site-header {
  background: white;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  z-index: 1000;
}

.nav-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.logo {
  font-size: 1.25rem;
  font-weight: 700;
  color: #111827;
}

.nav-links a {
  margin-left: 1.5rem;
  text-decoration: none;
  color: #374151;
  font-weight: 500;
}

.nav-links a:hover {
  color: #111827;
}

/* ===== Mobile Nav ===== */
.nav-toggle {
  display: none;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
    flex-direction: column;
    background: white;
    position: absolute;
    top: 60px;
    right: 0;
    width: 200px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  .nav-links.open {
    display: flex;
  }

  .nav-toggle {
    display: block;
  }
}
</style>