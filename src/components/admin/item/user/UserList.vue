<template>
  <div class="user-list">
    <div class="header">
      <h2>Daftar Data User</h2>
      <button class="add-btn" @click="openAddUserModal">
        <i class="bi bi-plus-square"></i> Tambah Pengguna
      </button>
    </div>
    <div class="table-responsive">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Username</th>
            <th>Email</th>
            <th>Role</th>
            <th class="action-column">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.role }}</td>
            <td class="action-column">
              <button class="edit-btn" @click="openEditUserModal(user)"><i class="bi bi-pencil-square"></i></button>
              <button class="delete-btn" @click="handleDelete(user.id)"><i class="bi bi-trash3"></i></button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Modal untuk Tambah/Edit Pengguna -->
    <ModalApp :visible="showModal" @close="showModal = false">
      <h3>{{ isEditMode ? 'Edit Pengguna' : 'Tambah Pengguna' }}</h3>
      <form @submit.prevent="handleSubmit">
        <div>
          <label>Username:</label>
          <input v-model="currentUser.username" type="text" required />
        </div>
        <div>
          <label>Email:</label>
          <input v-model="currentUser.email" type="email" required />
        </div>
        <div>
          <label>Role:</label>
          <select v-model="currentUser.role" required>
            <option value="Admin">Admin</option>
            <option value="User">User</option>
          </select>
        </div>
        <button type="submit">{{ isEditMode ? 'Update' : 'Tambah' }} Pengguna</button>
      </form>
    </ModalApp>
  </div>
</template>

<script>
import ModalApp from '@/components/ModalApp.vue';

export default {
  name: 'UserList',
  components: {
    ModalApp,
  },
  data() {
    return {
      users: [
        { id: 1, username: "Indah", email: "indah@gmail.com", role: "Admin" },
        { id: 2, username: "Mona", email: "Mona@gmail.com", role: "User" },
        { id: 3, username: "Dona", email: "Dona@gmail.com", role: "Admin" },
        { id: 4, username: "Jona", email: "Jona@gmail.com", role: "User" },
        { id: 5, username: "Dona", email: "Dona@gmail.com", role: "Admin" },
        { id: 6, username: "Jona", email: "Jona@gmail.com", role: "User" },
      ],
      showModal: false,
      isEditMode: false,
      currentUser: {
        id: null,
        username: '',
        email: '',
        role: 'User',
      },
    };
  },
  methods: {
    openAddUserModal() {
      this.isEditMode = false;
      this.currentUser = { id: null, username: '', email: '', role: 'User' };
      this.showModal = true;
    },
    openEditUserModal(user) {
      this.isEditMode = true;
      this.currentUser = { ...user };
      this.showModal = true;
    },
    handleSubmit() {
      if (this.isEditMode) {
        // Update user
        const index = this.users.findIndex(user => user.id === this.currentUser.id);
        if (index !== -1) this.users.splice(index, 1, this.currentUser);
      } else {
        // Add new user
        this.currentUser.id = this.users.length + 1;
        this.users.push({ ...this.currentUser });
      }
      this.showModal = false;
    },
    handleDelete(id) {
      if (confirm('Apakah Anda yakin ingin menghapus pengguna ini?')) {
        this.users = this.users.filter(user => user.id !== id);
        this.$emit('delete-user-success', id);
      }
    }
  },
};
</script>
<style scoped>
.user-list {
  width: 77%;
  padding: 1.5rem;
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.header h2 {
  color: #1f2937;
  font-size: 1.5rem;
  font-weight: 600;
  margin: 0;
}
h2{
  text-align: center;
}
.add-btn {
  background-color: #4f46e5;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: background-color 0.2s;
}

.add-btn:hover {
  background-color: #4338ca;
}

.table-responsive {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
  font-size: 0.875rem;
}

thead {
  background-color: #f9fafb;
}

th {
  text-align: left;
  padding: 0.75rem 1rem;
  color: #4b5563;
  font-weight: 600;
  border-bottom: 2px solid #e5e7eb;
}

td {
  padding: 0.75rem 1rem;
  color: #1f2937;
  border-bottom: 1px solid #e5e7eb;
}

tr:hover {
  background-color: #f9fafb;
}

.action-column {
  text-align: center;
  width: 200px;
}

.edit-btn, .delete-btn {
  padding: 0.375rem 0.75rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
  margin: 0 0.25rem;
  transition: all 0.2s;
}

.edit-btn {
  background-color: #10b981;
  color: white;
}

.edit-btn:hover {
  background-color: #059669;
}

.delete-btn {
  background-color: #ef4444;
  color: white;
}

.delete-btn:hover {
  background-color: #dc2626;
}

@media (max-width: 768px) {
  .user-list {
    padding: 1rem;
  }

  .header {
    flex-direction: column;
    gap: 1rem;
    align-items: stretch;
  }

  .header h2 {
    text-align: center;
  }

  .add-btn {
    width: 100%;
  }

  table {
    font-size: 0.813rem;
  }

  th,
  td {
    padding: 0.625rem;
  }

  .action-column {
    width: auto;
  }

  .edit-btn, .delete-btn {
    padding: 0.25rem 0.5rem;
    font-size: 0.813rem;
  }
}

form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

form div {
  margin-bottom: 15px;
}

label {
  font-size: 1rem;
  color: #333;
  margin-bottom: 5px;
  display: block;
}

input[type="text"],
input[type="email"],
select {
  width: 100%;
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 5px;
  font-size: 1rem;
  color: #333;
  box-sizing: border-box;
  transition: border-color 0.3s;
}

input[type="text"]:focus,
input[type="email"]:focus,
select:focus {
  border-color: #4f46e5;
  outline: none;
}

button[type="submit"] {
  width: 100%;
  padding: 12px;
  background-color: #4f46e5;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s;
}

button[type="submit"]:hover {
  background-color: #4338ca;
}

</style>