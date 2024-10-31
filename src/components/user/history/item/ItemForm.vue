<template>
  <div>
    <form @submit.prevent="submitForm" class="mb-3">
      <div class="mb-3">
        <label for="kode" class="form-label">Kode Barang</label>

        <input
          type="number"
          v-model="form.kode"
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
          v-model="form.nama"
          id="nama"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label for="deskripsi" class="form-label">Deskripsi</label>

        <input
          type="text"
          v-model="form.deskripsi"
          id="deskripsi"
          class="form-control"
          required
        />
      </div>

      <div class="mb-3">
        <label for="stok" class="form-label">Stok</label>

        <input
          type="number"
          v-model="form.stok"
          id="stok"
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
  name: "ItemForm",
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
        this.form = {
          code: newItem.code || "",
          name: newItem.name || "",
          description: newItem.description || "",
          stock: newItem.stock || 0,
        };
      },
    },
  },
  methods: {
    submitForm() {
      if (this.form.stock < 0) {
        alert("Stok tidak boleh negatif");
        return;
      }

      this.$emit("submit", { ...this.form });
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  max-width: 500px;
  margin: 20px auto;
}

td {
  padding: 10px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input:disabled {
  background-color: #f5f5f5;
}

button {
  padding: 8px 16px;
  background-color: #4f46e5;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #4338ca;
}
.button-group {
  display: flex;
  gap: 1rem;
}

.cancel {
  background-color: #dc2626;
}

.cancel:hover {
  background-color: #b91c1c;
}
</style>
