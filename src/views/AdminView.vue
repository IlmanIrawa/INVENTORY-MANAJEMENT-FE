<script>
import ItemList from "@/components/admin/item/ItemList.vue";
import ItemForm from "@/components/admin/item/ItemForm.vue";
import Transaction from "@/components/admin/item/transaction/Transaction.vue";
import UserList from "@/components/admin/item/user/UserList.vue";

export default {
  name: "AdminView",
  components: {
    ItemList,
    ItemForm,
    Transaction,
    UserList,
  },
  data() {
    return {
      showUserForm: false,
      selectedUser: null,
    };
  },
  props: {
    currentRole: {
      type: String,
      required: true,
    },
    isSidebarVisible: {
      type: Boolean,
      required: true,
    },
    currentComponent: {
      type: String,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
    showItemForm: {
      type: Boolean,
      required: true,
    },
    selectedItem: {
      type: Object,
      default: null,
    },
  },
  methods: {
    handleAddUser() {
      this.showUserForm = true;
      this.selectedUser = null;
    },
    handleEditUser(user) {
      this.showUserForm = true;
      this.selectedUser = user;
    },
    handleDeleteUserSuccess(userId) {
      // Implementasi notifikasi sukses jika diperlukan
      console.log(`User dengan ID ${userId} berhasil dihapus`);
    },
  },
  emits: ["update:show-item-form", "save-item", "edit-item", "delete-item"],
};
</script>

<template>
  <div class="admin-view">
    <!-- Users Management -->
    <div v-if="currentComponent === 'users'">
      <div class="component-container">
        <UserList
          @add-user="handleAddUser"
          @edit-user="handleEditUser"
          @delete-user-success="handleDeleteUserSuccess"
        />
      </div>
    </div>

    <!-- Items Management -->
    <div v-if="currentComponent === 'items'">
      <div class="component-container">
        <ItemForm
          v-if="showItemForm"
          :edit-item="selectedItem"
          @save-item="$emit('save-item', $event)"
          @cancel="$emit('update:show-item-form', false)"
        />
        <ItemList
          :items="items"
          @edit-item="$emit('edit-item', $event)"
          @delete-item="$emit('delete-item', $event)"
          @add-new="$emit('update:show-item-form', true)"
        />
      </div>
    </div>

    <!-- Transactions Management -->
    <div v-if="currentComponent === 'transactions'">
      <div class="component-container">
        <Transaction />
      </div>
    </div>
  </div>
</template>

<style scoped>
.admin-view {
  width: 100%;
  padding: 20px;
  margin-top: 60px; /* Space for header */
  margin-left: 240px; /* Space for sidebar */
  min-height: calc(100vh - 60px);
  background-color: #f5f5f5;
}

.component-container {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  padding: 0; /* Menghilangkan padding agar tidak dobel dengan padding dari UserList */
  min-height: 100%
}

@media (max-width: 576px) {
  .admin-view {
    margin-left: 0; /* Remove sidebar margin on mobile */
    padding: 15px;
  }
}
</style>