<template>
  <header class="header">
    <div class="header-left">
      <!-- Toggle Sidebar Button -->
      
      
      <!-- Logo/Brand -->
      <h1 class="brand">INVENTORY MANAJ</h1>
      <button 
        class="menu-button"
        @click="$emit('toggle-sidebar')"
      >
        <i class="bi bi-x-square-fill"></i>
      </button>
    </div>

    <!-- Search Bar -->
    <div class="search-container">
      <div class="search-box">
        <i class="fas fa-search search-icon"></i>
        <input 
          type="text"
          placeholder="Search..."
          v-model="searchQuery"
          @input="handleSearch"
          class="search-input"
        />
        <button 
          v-if="searchQuery"
          @click="clearSearch"
          class="clear-button"
        >
          <i class="fas fa-times"></i>
        </button>
      </div>
    </div>

    <div class="header-right">
      <!-- Role Selector -->
      <i class="bi bi-person-fill"></i>
      <select 
        class="role-select"
        :value="currentRole"
        @change="$emit('update-role', $event.target.value)"
      >
        <option value="user">User</option>
        <option value="admin">Admin</option>
      </select>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: {
    currentRole: {
      type: String,
      required: true
    },
    isSidebarVisible: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      searchQuery: ''
    }
  },
  methods: {
    handleSearch() {
      this.$emit('search', this.searchQuery)
    },
    clearSearch() {
      this.searchQuery = ''
      this.$emit('search', '')
    }
  },
  emits: ['update-role', 'toggle-sidebar', 'search']
}
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 70px;
  background-color: rgb(192, 199, 199);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
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
  border-radius: 5px;
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
.role-select{
  background-color: rgb(161, 196, 161);
  border-radius: 10%;
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