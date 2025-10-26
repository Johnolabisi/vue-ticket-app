<script setup>
import { clearSession, showToast } from "@/utils/helper";
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

// reactive refs for counts
const openCount = ref(0);
const progressCount = ref(0);
const closedCount = ref(0);
const recentTickets = ref([]);

// reusable function to safely load tickets
const getTickets = () => {
  try {
    return JSON.parse(localStorage.getItem("ticketapp_tickets")) || [];
  } catch (e) {
    console.error("Error parsing tickets:", e);
    return [];
  }
};

// when component mounts
onMounted(() => {
  const tickets = getTickets();

  // Count tickets by status
  openCount.value = tickets.filter((t) => t.status === "open").length;
  progressCount.value = tickets.filter(
    (t) => t.status === "in_progress"
  ).length;
  closedCount.value = tickets.filter((t) => t.status === "closed").length;

  // Recent 5 tickets
  recentTickets.value = tickets.slice(-5).reverse();
});
const router = useRouter();

function logout() {
  clearSession();
  showToast("You have been logged out.", "info");
  router.push("/login");
}
</script>

<template>
  <section class="dashboard-section">
    <aside class="sidebar">
      <div class="sidebar-header">
        <h2>TicketApp</h2>
      </div>
      <nav class="sidebar-nav">
        <a href="/dashboard" class="active">Dashboard</a>
        <a href="/tickets">Tickets</a>
        <a href="#" @click.prevent="logout">Logout</a>

      </nav>
    </aside>

    <main class="dashboard-main">
      <header class="dashboard-header">
        <h1>Welcome Back 👋</h1>
        <p>Here’s an overview of your tickets</p>
      </header>

      <div class="stats-grid">
        <div class="stat-card open">
          <h3>Open Tickets</h3>
          <p>{{ openCount }}</p>
        </div>

        <div class="stat-card progress">
          <h3>In Progress</h3>
          <p>{{ progressCount }}</p>
        </div>

        <div class="stat-card closed">
          <h3>Closed Tickets</h3>
          <p>{{ closedCount }}</p>
        </div>
      </div>

      <div class="recent-tickets">
        <h2>Recent Tickets</h2>

        <div v-if="recentTickets.length === 0" class="ticket-list">
          <p class="placeholder">
            No tickets found. Create one to get started!
          </p>
        </div>

        <div v-else class="ticket-list">
          <div
            v-for="ticket in recentTickets"
            :key="ticket.id"
            class="ticket-item"
            :class="ticket.status"
          >
            <h4>{{ ticket.title }}</h4>
            <p>{{ ticket.description || "No description provided." }}</p>
            <span class="status" :class="ticket.status">
              {{ ticket.status.replace("_", " ") }}
            </span>
          </div>
        </div>
      </div>
    </main>

    <div id="toast" class="toast"></div>
  </section>
</template>

<style scoped>
/* ===== DASHBOARD ===== */
.dashboard-section {
  display: flex;
  min-height: 100vh;
  background: #f9fafb;
}

/* Sidebar */
.sidebar {
  width: 250px;
  background: #1e3a8a;
  color: white;
  display: flex;
  flex-direction: column;
  padding: 1.5rem;
}

.sidebar-header h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 2rem;
  text-align: center;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.sidebar-nav a {
  color: #e0e7ff;
  text-decoration: none;
  font-weight: 500;
  padding: 0.6rem 1rem;
  border-radius: 8px;
  transition: background 0.3s ease;
}

.sidebar-nav a:hover,
.sidebar-nav a.active {
  background: rgba(255, 255, 255, 0.15);
}

/* Main */
.dashboard-main {
  flex: 1;
  padding: 2rem;
}

.dashboard-header h1 {
  font-size: 1.8rem;
  color: #111827;
}

.dashboard-header p {
  color: #6b7280;
  margin-bottom: 2rem;
}

/* Stats Cards */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.stat-card {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);
  text-align: center;
}

.stat-card h3 {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: #374151;
}

.stat-card p {
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
}

.stat-card.open p {
  color: #2563eb;
}

.stat-card.progress p {
  color: #f59e0b;
}

.stat-card.closed p {
  color: #16a34a;
}

/* Recent Tickets */
.recent-tickets h2 {
  font-size: 1.3rem;
  margin-bottom: 1rem;
  color: #1e3a8a;
}

.ticket-list {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);
  min-height: 120px;
}

.placeholder {
  color: #6b7280;
  text-align: center;
}

/* Responsive */
@media (max-width: 900px) {
  .sidebar {
    display: none;
  }

  .dashboard-main {
    padding: 1rem;
  }
}
</style>
