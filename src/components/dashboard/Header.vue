<template>
  <header class="header">
    <div class="header-left">
      <!-- Toggle Sidebar Button -->
      <button class="menu-button" @click="$emit('toggle-sidebar')">
        <i class="fas fa-bars"></i>
      </button>

      <!-- Logo/Brand -->
      <h1 class="brand">Inventory Manaj</h1>
    </div>

    <!-- Search Bar -->
    <div class="search-container">
      <div class="search-box">
        <i class="fas fa-search search-icon"></i>
        <input
          type="text"
          placeholder="Search inventory..."
          v-model="searchQuery"
          @input="emitSearch"
          class="search-input"
        />
        <button v-if="searchQuery" @click="clearSearch" class="clear-button">
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>

    <div class="header-right">
      <!-- Role Selector -->
      <select
        class="role-select"
        :value="currentRole"
        @change="selectRole($event.target.value)"
      >
        <option value="user">PH Operator</option>
        <option value="admin">WH Operator</option>
      </select>

      <!-- User Profile -->
      <div class="user-profile">
        <i class="fas fa-user"></i>
      </div>
    </div>

    <!-- logout-->
    <div class="logout-container">
      <button class="logout-btn btn btn-outline-light" @click="logout">
        Logout
      </button>
    </div>
  </header>
</template>

<script>
import { useAuthStore } from "@/store/authStore";
import EventBus from "@/utils/EventBus";

export default {
  data() {
    return {
      searchQuery: "", // Untuk search bar
    };
  },

  computed: {
    currentRole() {
      const authStore = useAuthStore();
      return authStore.role; // Ambil langsung dari store Pinia
    },
  },

  methods: {
    clearSearch() {
      this.searchQuery = "";
      this.emitSearch(); // Emit event kosong
    },

    emitSearch() {
      EventBus.emit("search", this.searchQuery);
    },

    selectRole(role) {
      const authStore = useAuthStore();
      const authRole = authStore.role; // Gunakan state dari Pinia
      const isAuthenticated = Boolean(localStorage.getItem("auth"));

      if (isAuthenticated && authRole === role) {
        this.$router.push({ name: role, params: { component: "items" } });
      } else {
        alert("You do not have permission to switch to this role.");
        this.$router.push({ name: "login" });
        this.$emit("toggle-sidebar", false);
      }
    },

    logout() {
      const authStore = useAuthStore();
      authStore.logout(); // Logout dari store Pinia
      this.$router.push({ name: "login" });
    },
  },
};
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 70px;
  background-color: rgb(195, 202, 206);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  z-index: 1000;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 20px;
  min-width: 200px;
}

.menu-button {
  background: none;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  padding: 8px;
  border-radius: 4px;
}

.menu-button:hover {
  background-color: #f0f0f0;
}

.brand {
  font-size: 1.2rem;
  font-weight: 600;
  color: #333;
}

/* Search Bar Styles */
.search-container {
  flex: 1;
  max-width: 600px;
  margin: 0 20px;
}

.search-box {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.search-input {
  width: 100%;
  padding: 10px 40px;
  border: 1px solid #ddd;
  font-size: 0.95rem;
  transition: all 0.3s ease;
}

.search-input:focus {
  outline: none;
  border-color: #4a90e2;
  box-shadow: 0 0 0 2px rgba(74, 144, 226, 0.1);
}

.search-icon {
  position: absolute;
  left: 15px;
  color: #666;
}

.clear-button {
  position: absolute;
  right: 15px;
  background: none;
  border: none;
  color: #666;
  cursor: pointer;
  padding: 0;
}

.clear-button:hover {
  color: #333;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 20px;
  min-width: 200px;
  justify-content: flex-end;
}

.role-select {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ddd;
  background-color: white;
  cursor: pointer;
}

.user-profile {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.user-profile:hover {
  background-color: #e0e0e0;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .search-container {
    max-width: none;
    margin: 0 10px;
  }

  .header-left {
    min-width: auto;
  }

  .header-right {
    min-width: auto;
  }

  .brand {
    display: none;
  }
}

@media (max-width: 576px) {
  .header {
    padding: 0 10px;
  }

  .search-input {
    padding: 8px 35px;
  }
}
</style>
