<script setup>
import { getUsers, getSession, setSession, showToast } from "@/utils/helper";
import { ref } from "vue";
const email = ref("");
const password = ref("");
function handleSubmit() {
  if (!email.value || !password.value) {
    showToast("Both email and password are required.", "error");
    return;
  }

  const users = getUsers();
  const user = users.find((u) => u.email === email.value && u.password === password.value);
  if (!user) {
    showToast("Invalid email or password. Please try again.", "error");
    return;
  }

  setSession(email.value);
  showToast("Login successful! Redirecting...", "success");
  setTimeout(() => (window.location.href = "/dashboard"), 1200);
}
</script>

<template>
    <section class="auth-section">
  <div class="container auth-container">
    <div class="auth-box">
      <h2>Welcome Back</h2>
      <p class="subtitle">Login to access your dashboard</p>

      <form id="loginForm" class="auth-form" @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="email">Email</label>
          <input
            v-model="email"
            type="email"
            id="email"
            name="email"
            placeholder="you@example.com"
            required
          />
          <small class="error-message" id="emailError"></small>
        </div>

        <div class="form-group">
          <label for="password">Password</label>
          <input
            v-model="password"
            type="password"
            id="password"
            name="password"
            placeholder="Enter your password"
            required
          />
          <small class="error-message" id="passwordError"></small>
        </div>

        <button type="submit" class="btn primary full">Login</button>
      </form>

      <p class="auth-switch">
        Don’t have an account? <a href="/signup">Sign up</a>
      </p>
    </div>
  </div>

  <!-- Toast Notification -->
  <div id="toast" class="toast"></div>
</section>
</template>
<style scoped>
.auth-section {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 90vh;
  background: linear-gradient(135deg, #2563eb 0%, #1e3a8a 100%);
  padding: 2rem 1rem;
  color: #111;
}

.auth-container {
  width: 100%;
  max-width: 420px;
}

.auth-box {
  background: white;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  padding: 2.5rem;
  text-align: center;
}

.auth-box h2 {
  margin-bottom: 0.5rem;
  font-size: 1.75rem;
  color: #1e3a8a;
}

.auth-box .subtitle {
  color: #6b7280;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}

.auth-form .form-group {
  text-align: left;
  margin-bottom: 1.25rem;
}

.auth-form label {
  display: block;
  margin-bottom: 0.4rem;
  font-weight: 600;
  color: #374151;
}

.auth-form input {
  width: 100%;
  padding: 0.75rem;
  border-radius: 8px;
  border: 1px solid #d1d5db;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

.auth-form input:focus {
  border-color: #2563eb;
  outline: none;
}

.btn.full {
  width: 100%;
  margin-top: 1rem;
  font-size: 1rem;
}

.auth-switch {
  margin-top: 1rem;
  color: #4b5563;
  font-size: 0.9rem;
}

.auth-switch a {
  color: #2563eb;
  text-decoration: none;
  font-weight: 600;
}

.auth-switch a:hover {
  text-decoration: underline;
}

/* ===== Error & Toast Styles ===== */



</style>