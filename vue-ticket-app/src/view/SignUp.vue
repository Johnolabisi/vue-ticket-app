<script setup>
import { getUsers, saveUsers, showToast } from "@/utils/helper";
import { ref } from "vue";

const name = ref("");
const email = ref("");
const password = ref("");

function handleSubmit() {
  if (!email.value || !password.value || !name.value) {
    showToast("All fields are required.", "error");
    return;
  }

  const users = getUsers();
  if (users.find((u) => u.email === email)) {
    showToast("Email already exists. Please log in.", "error");
    return;
  }

  users.push(
    users.push({
      name: name.value,
      email: email.value,
      password: password.value,
    })
  );
  saveUsers(users);
  showToast("Signup successful! Please log in.", "success");
  setTimeout(() => (window.location.href = "/login"), 1200);
}
</script>

<template>
  <section class="auth-section">
    <div class="container auth-container">
      <div class="auth-box">
        <h2>Create an Account</h2>
        <p class="subtitle">Join TicketApp to manage tickets easily</p>

        <form id="signupForm" class="auth-form" @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="name">Full Name</label>
            <input
              v-model="name"
              type="text"
              id="name"
              name="name"
              placeholder="John Doe"
              required
            />
            <small class="error-message" id="nameError"></small>
          </div>

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
              placeholder="Enter a strong password"
              required
            />
            <small class="error-message" id="passwordError"></small>
          </div>

          <button type="submit" class="btn primary full">Sign Up</button>
        </form>

        <p class="auth-switch">
          Already have an account? <a href="/login">Login</a>
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
</style>
