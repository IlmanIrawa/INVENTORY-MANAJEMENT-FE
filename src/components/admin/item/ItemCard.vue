<template>
  <div class="item-card card mb-3">
    <div class="card-body">
      <h3 class="card-title">{{ item.name }}</h3>
      <p class="card-text">{{ item.description }}</p>
      <p class="card-text stock">Stok: {{ item.stock }}</p>
      <div class="buttons d-flex gap-2 mt-3">
        <button @click="editItem" class="btn btn-success edit">
          <i class="bi bi-pencil-square"></i> Edit
        </button>
        <button
          @click="$emit('delete-item', item.code)"
          class="btn btn-danger delete"
        >
          <i class="bi bi-trash3"></i> Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ItemCard",
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  methods: {
    editItem() {
      this.$emit("edit-item", this.item);
    },
  },
};
</script>


<style scoped>
.item-card {
transition: transform 0.2s, box-shadow 0.2s;
}
.item-card:hover {
transform: translateY(-5px);
box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
.item-card .card-title {
margin: 0 0 10px;
font-size: 1.25rem;
color: #333;
}
.item-card .card-text {
margin: 5px 0;
color: #555;
}
.item-card .stock {
font-weight: bold;
color: #2c3e50;
}
.item-card button {
transition: background-color 0.2s;
}
</style>