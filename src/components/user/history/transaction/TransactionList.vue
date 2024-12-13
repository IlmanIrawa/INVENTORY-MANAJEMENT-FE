<template>
  <div class="user-list">
    <div class="header">
      <h2>Daftar Transaksi</h2>
    </div>
    
    <div class="table-responsive">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nama Karyawan</th>
            <th>Nama Barang</th>
            <th>Jumlah Pinjam</th>
            <th>Tanggal Pinjam</th>
            <th>Tanggal Kembali</th>
            <th>Status</th>
            <th class="action-column">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="transaction in transactions" :key="transaction.kode">
            <td>{{ transaction.kode }}</td>
            <td>{{ transaction.namaKaryawan }}</td>
            <td>{{ transaction.namaBarang }}</td>
            <td>{{ transaction.jumlahPinjam }}</td>
            <td>{{ transaction.tanggalPinjam }}</td>
            <td>{{ transaction.tanggalKembali }}</td>
            <td>
              <span 
                class="status-badge"
                :class="{ 'status-borrowed': transaction.status === 'Borrowed', 'status-returned': transaction.status === 'Returned' }"
              >
                {{ transaction.status }}
              </span>
            </td>
            <td class="action-buttons">
              <button
                v-if="transaction.status === 'Borrowed'"
                class="return-btn"
                @click="$emit('return-item', transaction)"
              >
                <i ></i>
                <i class="bi bi-arrow-counterclockwise"></i>Kembalikan
              </button>
              <span v-else class="returned-text">Sudah Dikembalikan</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "TransactionList",
  data() {
    return {
      transactions: [
       {
          kode: "1",
          namaKaryawan: "Mona",
          namaBarang: "Printer",
          jumlahPinjam: 1,
          tanggalPinjam: "1888-10-10",
          tanggalKembali: "1999-10-10",
          status: "Borrowed",
        },
        {
          kode: "2",
          namaKaryawan: "Jona",
          namaBarang: "Tas Sekolah",
          jumlahPinjam: 2,
          tanggalPinjam: "2000-10-01",
          tanggalKembali: "2000-11-10",
          status: "Borrowed",
        },
        {
          kode: "3",
          namaKaryawan: "Ilman",
          namaBarang: "Laptop",
          jumlahPinjam: 1,
          tanggalPinjam: "2023-12-10",
          tanggalKembali: "2023-10-10",
          status: "Borrowed",
        },
        {
          kode: "4",
          namaKaryawan: "Ilhamm",
          namaBarang: "Laptop",
          jumlahPinjam: 12,
          tanggalPinjam: "2020-12-10",
          tanggalKembali: "2023-10-10",
          status: "Borrowed",
        },
        {
          kode: "3",
          namaKaryawan: "Ilman",
          namaBarang: "Laptop",
          jumlahPinjam: 1,
          tanggalPinjam: "2023-12-10",
          tanggalKembali: "2023-10-10",
          status: "Borrowed",
        },
      ],
    };
  },
  methods: {
    verifikasi(transaction) {
      const index = this.transactions.findIndex(
        (t) => t.kode === transaction.kode
      );

      if (index !== -1) {
        this.transactions[index].status = "Returned";
        this.$emit("transaction-verified", transaction.kode);
      }
    },
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

/* Status Badge Styles */
.status-badge {
  padding: 0.25rem 0.75rem;
  font-size: 0.75rem;
  font-weight: 500;
}

.status-borrowed {
  background-color: #7464bb;
  color: #101111;
}

.status-returned {
  background-color: #D1FAE5;
  color: #4c60b9;
}

/* Return Button Styles */
.return-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background-color: #3B82F6;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.return-btn:hover {
  background-color: #2563EB;
}

.return-btn:active {
  transform: scale(0.98);
}

.return-btn i {
  font-size: 0.875rem;
}

.returned-text {
  color: #6B7280;
  font-size: 0.875rem;
  font-style: italic;
}

.action-buttons {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
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

  .return-btn {
    padding: 0.375rem 0.75rem;
    font-size: 0.813rem;
  }
}
</style>