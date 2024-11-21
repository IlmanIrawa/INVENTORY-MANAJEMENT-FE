<template>
  <div id="app">
    <Header
      v-show="showHeader"
      :currentRole="currentRole"
      @update-role="updateRole"
      @toggle-sidebar="toggleSidebar"
      :isSidebarVisible="isSidebarVisible"
    />

    <div class="app-content" :class="{ noHeader: !showHeader }">
      <Sidebar
        v-show="showSidebar"
        :currentRole="currentRole"
        :isSidebarVisible="isSidebarVisible"
        @showComponent="navigateTo"
        :currentComponent="$route.params.component" 
      />

      <div
        class="main-content"
        :class="{ expanded: isSidebarVisible && showSidebar }"
      >
        <router-view
          :key="$route.fullPath"
          :currentComponent="$route.params.component"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/dashboard/Header.vue";
import Sidebar from "./components/dashboard/Sidebar.vue";
import EventBus from "./utils/EventBus";

export default {
  components: {
    Header,
    Sidebar,
  },

  data() {
    return {
      // Mengambil currentRole dari params jika ada, atau default "admin"
      currentRole: this.$route.params.role || "admin",
      isSidebarVisible: true,
      searchTerm: "",
    };
  },

  computed: {
    showHeader() {
      // Menyesuaikan apakah header harus ditampilkan berdasarkan metadata route
      return this.$route.meta?.hideHeader !== true;
    },

    showSidebar() {
      // Menyesuaikan apakah sidebar harus ditampilkan berdasarkan metadata route
      return this.$route.meta?.hideSidebar !== true;
    },
  },

  watch: {
    // Mengupdate currentRole setiap kali rute berubah
    "$route.name"(newRole) {
      this.currentRole = newRole;
    },
  },

  methods: {
    updateRole(role) {
      this.currentRole = role;
    },

    navigateTo(component) {
      // Navigasi berdasarkan role dan komponen
      if (this.currentRole === "ADMIN" && component) {
        this.$router.push({ name: "admin", params: { component } });
      } else if (this.currentRole === "USER") {
        this.$router.push({ name: "user" });
      } else {
        this.$router.push({ name: "login" });
      }
    },

    toggleSidebar() {
      this.isSidebarVisible = !this.isSidebarVisible;
    },

    handleSearch(newQuery) {
      console.log("Search term:", newQuery);

      if (this.currentRole === "admin") {
        console.log("Search in admin items");
      } else if (this.currentRole === "user") {
        console.log("Search in user items");
      }
    },
  },

  mounted() {
    // Menghubungkan event search dengan EventBus
    EventBus.on("search", this.handleSearch);
  },

  beforeUnmount() {
    // Melepaskan event search ketika komponen di-unmount
    EventBus.off("search", this.handleSearch);
  },
};
</script>

<style scoped>
.app-content.noHeader .main-content {
  margin-top: 0; /* Menyesuaikan padding/top untuk konten tanpa header */
}

.app-content .main-content.expanded {
  margin-left: 250px; /* Menyesuaikan margin ketika sidebar terlihat */
}
</style>
