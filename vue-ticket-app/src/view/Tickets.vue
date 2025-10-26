<script setup>
import { ref, onMounted } from "vue";
import { clearSession, showToast } from "@/utils/helper"; // assuming you have this helper
import { useRouter } from "vue-router";

// ===== Reactive State =====
const tickets = ref([]);
const newTicket = ref({ title: "", description: "", status: "open" });
const isEditModalVisible = ref(false);
const editTicket = ref({ id: null, title: "", description: "", status: "open" });

// ===== Helpers =====
const getTickets = () => JSON.parse(localStorage.getItem("ticketapp_tickets")) || [];
const saveTickets = (data) => localStorage.setItem("ticketapp_tickets", JSON.stringify(data));

const renderTickets = () => {
  tickets.value = getTickets();
};

// ===== Create Ticket =====
function addTicket() {
  if (!newTicket.value.title || !newTicket.value.status) {
    showToast("Title and status are required.", "error");
    return;
  }

  const updated = [
    ...getTickets(),
    {
      id: Date.now(),
      title: newTicket.value.title.trim(),
      description: newTicket.value.description.trim(),
      status: newTicket.value.status,
    },
  ];

  saveTickets(updated);
  renderTickets();
  showToast("Ticket added successfully!", "success");
  newTicket.value = { title: "", description: "", status: "open" };
}

// ===== Edit Ticket =====
function openEditModal(ticket) {
  editTicket.value = { ...ticket };
  isEditModalVisible.value = true;
}

function saveEdit() {
  if (!editTicket.value.title || !["open", "in_progress", "closed"].includes(editTicket.value.status)) {
    showToast("Invalid input or status.", "error");
    return;
  }

  const updated = getTickets().map((t) =>
    t.id === editTicket.value.id ? { ...editTicket.value } : t
  );

  saveTickets(updated);
  renderTickets();
  isEditModalVisible.value = false;
  showToast("Ticket updated successfully!", "success");
}

function cancelEdit() {
  isEditModalVisible.value = false;
}

// ===== Delete Ticket =====
function deleteTicket(id) {
  if (confirm("Are you sure you want to delete this ticket?")) {
    const updated = getTickets().filter((t) => t.id !== id);
    saveTickets(updated);
    renderTickets();
    showToast("Ticket deleted successfully.", "info");
  }
}
const router = useRouter();

function logout() {
  clearSession();
  showToast("You have been logged out.", "info");
  router.push("/login");
}
// ===== On mount =====
onMounted(() => {
  renderTickets();
});
</script>


<template>
  <section class="tickets-page container">
    <h1>Ticket Management</h1>

    <!-- ===== Create Ticket Form ===== -->
    <div class="ticket-form">
      <h2>Create New Ticket</h2>
      <form @submit.prevent="addTicket">
        <div class="form-group">
          <label>Title *</label>
          <input v-model="newTicket.title" type="text" placeholder="Enter ticket title" required />
        </div>

        <div class="form-group">
          <label>Description</label>
          <textarea v-model="newTicket.description" placeholder="Enter description (optional)"></textarea>
        </div>

        <div class="form-group">
          <label>Status *</label>
          <select v-model="newTicket.status" required>
            <option value="open">Open</option>
            <option value="in_progress">In Progress</option>
            <option value="closed">Closed</option>
          </select>
        </div>

        <button type="submit" class="btn-primary">Add Ticket</button>
      </form>
    </div>

    <hr />

    <!-- ===== Ticket List ===== -->
    <div class="ticket-list">
      <h2>Existing Tickets</h2>
      <div v-if="tickets.length" class="ticket-grid">
        <div v-for="ticket in tickets" :key="ticket.id" class="ticket-card" :class="ticket.status">
          <div class="ticket-info">
            <h3>{{ ticket.title }}</h3>
            <p>{{ ticket.description || "No description provided." }}</p>
          </div>

          <div class="ticket-footer">
            <span class="status" :class="ticket.status">
              {{ ticket.status.replace("_", " ") }}
            </span>

            <div class="ticket-actions">
              <button class="edit-btn" @click="openEditModal(ticket)">Edit</button>
              <button class="delete-btn" @click="deleteTicket(ticket.id)">Delete</button>
            </div>
          </div>
        </div>
      </div>
      <p v-else class="no-tickets">No tickets yet.</p>
    </div>

    <button id="logoutBtn" class="logout-btn" @click.prevent="logout">Logout</button>
  </section>

  <!-- ===== Edit Ticket Modal ===== -->
  <div v-if="isEditModalVisible" class="modal-overlay">
    <div class="modal">
      <h2>Edit Ticket</h2>
      <form @submit.prevent="saveEdit">
        <div class="form-group">
          <label>Title</label>
          <input v-model="editTicket.title" required />
        </div>

        <div class="form-group">
          <label>Description</label>
          <textarea v-model="editTicket.description" rows="4" required></textarea>
        </div>

        <div class="form-group">
          <label>Status</label>
          <select v-model="editTicket.status" required>
            <option value="open">Open</option>
            <option value="in_progress">In Progress</option>
            <option value="closed">Closed</option>
          </select>
        </div>

        <div class="modal-actions">
          <button type="button" class="btn-secondary" @click="cancelEdit">Cancel</button>
          <button type="submit" class="btn-primary">Save Changes</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Toast -->
  <div id="toast" class="toast"></div>
</template>

<style scoped>
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
/* ===== Layout Containers ===== */
.tickets-page {
  max-width: 1440px;
  margin: 2rem auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

/* ===== Section Boxes ===== */
.ticket-form,
.ticket-list {
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 18px rgba(0, 0, 0, 0.08);
  padding: 2rem;
}

.ticket-form h2,
.ticket-list h2 {
  margin-bottom: 1.5rem;
  font-size: 1.4rem;
  font-weight: 600;
  color: #222;
}

/* ===== Form Styling ===== */
.ticket-form form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.form-group label {
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group textarea,
.form-group select {
  padding: 0.8rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border 0.2s ease;
}

.form-group input:focus,
.form-group textarea:focus,
.form-group select:focus {
  border-color: #007bff;
}
.ticket-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

/* ===== Ticket Cards ===== */
.ticket-card {
  background: #f9fafc;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.06);
  position: relative;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.ticket-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
}

.ticket-card h3 {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  color: #222;
}

.ticket-card p {
  font-size: 0.95rem;
  color: #555;
  margin-bottom: 1rem;
}



/* ===== Actions ===== */
.ticket-actions {
  margin-top: 1rem;
  display: flex;
  gap: 0.5rem;
}

.ticket-actions button {
  padding: 0.5rem 0.9rem;
  font-size: 0.9rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.edit-btn {
  background: #ffc107;
  color: #333;
}

.edit-btn:hover {
  background: #e0a800;
}

.delete-btn {
  background: #dc3545;
  color: #fff;
}

.delete-btn:hover {
  background: #b02a37;
}

/* ===== Logout Button ===== */
.logout-btn {
  background: #6c757d;
  color: #fff;
  border: none;
  padding: 0.8rem 1rem;
  border-radius: 8px;
  align-self: flex-end;
  cursor: pointer;
  transition: background 0.3s ease;
}

.logout-btn:hover {
  background: #565e64;
}
@media (max-width: 768px) {
  .ticket-form, .ticket-list {
    padding: 1rem;
  }

  .ticket-card {
    padding: 1rem;
  }

  .tickets-page {
    padding: 0.5rem;
  }

  .logout-btn {
    width: 100%;
  }
}
/* ===== Modal Overlay ===== */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(15, 15, 15, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
  opacity: 1;
  transition: opacity 0.3s ease;
}

.modal-overlay.hidden {
  opacity: 0;
  pointer-events: none;
}

/* ===== Modal Box ===== */
.modal {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 480px;
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.25);
  animation: popIn 0.3s ease;
}

@keyframes popIn {
  from {
    transform: translateY(30px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.modal h2 {
  margin-bottom: 1.2rem;
  font-size: 1.3rem;
  font-weight: 600;
  color: #222;
  text-align: center;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 1.2rem;
}

.btn-secondary {
  background: #e9ecef;
  color: #333;
  border: none;
  padding: 0.7rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.btn-secondary:hover {
  background: #d6d8db;
}
</style>