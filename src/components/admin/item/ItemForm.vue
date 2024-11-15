<!-- ItemForm.vue -->
<template>
  <div>
    <form @submit.prevent="submitForm" class="mb-3">
      <div class="mb-3">
        <label for="kode" class="form-label">Kode Barang</label>
        <input
          type="number"
          v-model="form.code"
          id="kode"
          class="form-control"
          :disabled="isEdit"
          required
        />
      </div>
      <div class="mb-3">
        <label for="name" class="form-label">Nama Barang</label>
        <input
          type="text"
          v-model="form.name"
          id="name"
          class="form-control"
          required
        />
      </div>
      <div class="mb-3">
        <label for="description" class="form-label">Deskripsi</label>
        <input
          type="text"
          v-model="form.description"
          id="description"
          class="form-control"
          required
        />
      </div>
      <div class="mb-3">
        <label for="stock" class="form-label">Stok</label>
        <input
          type="number"
          v-model="form.stock"
          id="stock"
          class="form-control"
          required
        />
      </div>
      <button type="submit" class="btn btn-success">
        {{ isEdit ? "Simpan Perubahan" : "Tambah Barang" }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      default: () => ({
        code: "",
        name: "",
        description: "",
        stock: 0,
      }),
    },
    isEdit: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      form: {
        code: "",
        name: "",
        description: "",
        stock: 0,
      },
    };
  },
  watch: {
    item: {
      immediate: true,
      handler(newItem) {
        this.form = { ...newItem }; // Sinkronkan form dengan data yang diberikan
      },
    },
  },
  methods: {
    submitForm() {
      if (this.form.stock < 0) {
        alert("Stok tidak boleh negatif");
        return;
      }

      this.$emit("submit", { ...this.form }); // Emit event submit dengan data form
    },
  },
};
</script>

<style scoped>
form {
  max-width: 600px;
  margin: 0 auto;
}
input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
button {
  padding: 8px 16px;
  background-color: #4f46e5;
  color: white;
  border: none;
  border-radius: 4px;
}
button:hover {
  background-color: #4338ca;
}
</style>
