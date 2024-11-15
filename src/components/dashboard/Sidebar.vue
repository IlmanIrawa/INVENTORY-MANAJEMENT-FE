// src/components/dashboard/Sidebar.vue
/* eslint-disable */

<template>
  <aside :class="['sidebar', { 'sidebar-hidden': !isSidebarVisible }]">
    <!-- Menu untuk Admin -->
    <div v-if="currentRole === 'admin'" class="menu-list">
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'users' }"
        @click="$emit('show-component', 'users')"
      >
        <i class="bi bi-person-vcard"></i>
        <span>Data User</span>
      </div>
      
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'items' }"
        @click="$emit('show-component', 'items')"
      >
        <i class="bi bi-calendar4-event"></i>
        <span>Daftar Item</span>
      </div>
      
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'transactions' }"
        @click="$emit('show-component', 'transactions')"
      >
        <i class="bi bi-wallet2"></i>
        <span>Daftar Transaction</span>
      </div>
      
      
    </div>

    <!-- Menu untuk User -->
    <div v-else class="menu-list">
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'items' }"
        @click="$emit('show-component', 'items')"
      >
         <i class="bi bi-calendar4-event"></i>
        <span>Daftar Item</span>
      </div>
      
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'transactions' }"
        @click="$emit('show-component', 'transactions')"
      >
        <i class="bi bi-wallet2"></i>
        <span>Daftar Transaction</span>
      </div>
      
      <div 
        class="menu-item"
        :class="{ active: currentComponent === 'history' }"
        @click="$emit('show-component', 'history')"
      >
        <i class="fas fa-history"></i>
        <span>Riwayat Transaction</span>
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    currentRole: {
      type: String,
      required: true
    },
    isSidebarVisible: {
      type: Boolean,
      required: true
    },
    currentComponent: {
      type: String,
      required: true
    }
  },
  emits: ['show-component']
}
</script>


<style scoped>
.sidebar {
  position: fixed;
  top: 70px; 
  left: 0;
  bottom: 0;
  width: 250px;
  background-color: rgb(195, 202, 206);
  box-shadow: 2px 0 5px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
  z-index: 100;
}

.sidebar-hidden {
  transform: translateX(-100%);
}

.menu-list {
  padding: 1rem 0;
}

.menu-item {
  display: flex;
  align-items: center;
  padding: 0.75rem 1.5rem;
  color: #333;
  cursor: pointer;
  transition: all 0.2s ease;
}

.menu-item:hover {
  background-color: #f8f9fa;
  color: #4f46e5;
}

.menu-item.active {
  background-color: #4f46e5;
  color: white;
}

.menu-item i {
  width: 20px;
  margin-right: 10px;
}

@media (max-width: 768px) {
  .sidebar {
    width: 200px;
  }

  .menu-item {
    padding: 0.75rem 1rem;
  }
}
</style>