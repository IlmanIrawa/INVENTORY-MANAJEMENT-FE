<template>
  <div class="item-list container py-4">
    <div class="header d-flex justify-content-between align-items-center mb-3">
      <h2>Daftar Barang</h2>

      <button class="btn btn-primary" @click="showAddForm">
        <i class="bi bi-clipboard-plus-fill"></i> Tambah Item
      </button>
    </div>

    <div class="item-cards row">
      <ItemCard
        v-for="item in items"
        :key="item.code"
        :item="item"
        @edit-item="editItem" 
        @delete-item="handleDelete"
        class="col-md-3 col-lg-4"
      />
    </div>

    <Modal :visible="showForm" @close="cancelEditForm">
      <ItemForm
        :item="selectedItem"
        :isEdit="isEdit"
        @submit="handleSubmit"
        @cancel="cancelEditForm"
      />
    </Modal>
  </div>
</template>

<script>
import ItemCard from "./ItemCard.vue";
import Modal from "@/components/ModalApp.vue";
import ItemForm from "./ItemForm.vue";

export default {
  components: {
    ItemCard,
    Modal,
    ItemForm,
  },
  name: "ItemList",
  data() {
    return {
      items: [
        {
          code: "1",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },
        {
          code: "2",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },
        {
          code: "3",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },
        {
          code: "4",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },
        {
          code: "5",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },
        {
          code: "6",
          name: "Printer",
          description: "Ini adalah printer Canon",
          stock: 10,
        },

        // Data lainnya
      ],
      showForm: false,
      selectedItem: null,
      isEdit: false,
    };
  },
  methods: {
    showAddForm() {
      this.selectedItem = {
        code: "",
        name: "",
        description: "",
        stock: "",
      };
      this.isEdit = false;
      this.showForm = true;
    },

    editItem(item) {
      this.selectedItem = { ...item };
      this.isEdit = true;
      this.showForm = true;
    },

    handleSubmit(formData) {
      if (this.isEdit) {
        const index = this.items.findIndex(
          (item) => item.code === this.selectedItem.code
        );
        if (index !== -1) {
          this.items.splice(index, 1, { ...formData });
        }
      } else {
        this.items.push({ ...formData });
      }
      this.showForm = false;
      this.selectedItem = null;
      this.isEdit = false;
    },

    cancelEditForm() {
      this.showForm = false;
      this.selectedItem = null;
      this.isEdit = false;
    },

    handleDelete(code) {
      if (confirm("Apakah Anda yakin ingin menghapus barang ini?")) {
        this.items = this.items.filter((item) => item.code !== code);
      }
    },
  },
};
</script>


<style scoped>
.item-list{
  background-color: white;
  border-radius: 8px;
  margin: auto;
  box-shadow:0 0 10px rgba(red, green, blue, alpha) ;
 
}
.header h2{
  color: rgb(192, 199, 199);
  font-size: 24px;
}
.container py-4{
  width: 70%;
}
</style>
