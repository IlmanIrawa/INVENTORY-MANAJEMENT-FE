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
            <td>{{ transaction.status }}</td>
            <td class="action-buttons">
              
              <button

                class="verify-btn"
                @click="verifikasi(transaction)"
                :disabled="transaction.status === 'ACC'"
                :class="{ 'returned-btn': transaction.status === 'ACC' }"
              >
                {{
                  transaction.status === "ACC"
                    ? "Pengajuan di ACC"
                    : "Verifikasi"
                }}
              </button>
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
          kode: "5",
          namaKaryawan: "Ilman",
          namaBarang: "Laptop",
          jumlahPinjam: 1,
          tanggalPinjam: "2023-12-10",
          tanggalKembali: "2023-10-10",
          status: "Borrowed",
        },
        {
          kode: "6",
          namaKaryawan: "Ilhamm",
          namaBarang: "Laptop",
          jumlahPinjam: 12,
          tanggalPinjam: "2020-12-10",
          tanggalKembali: "2023-10-10",
          status: "Borrowed",
        },
      ],
    };
  },
  methods: {
    verifikasi(transaction) {
      // Mencari index transaksi yang akan diverifikasi
      const index = this.transactions.findIndex(
        (t) => t.kode === transaction.kode
      );

      if (index !== -1) {
        // Update status transaksi menjadi "Returned"
        this.transactions[index].status = "ACC";

        // Optional: Emit event ke parent component jika diperlukan
        this.$emit("transaction-verified", transaction.kode);
      }
    },
  },
};
</script>

<style scoped>
.user-list {
  width:77%;
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

.add-btn {
  background-color: #4f46e5;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: background-color 0.2s;
}

.add-btn:hover {
  background-color: #4338ca;
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

.verify-btn {
  padding: 0.375rem 0.75rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
  background-color: #10b981;
  color: white;
  transition: all 0.2s;
}

.verify-btn:hover:not(:disabled) {
  background-color: #059669;
}

.verify-btn:disabled {
  background-color: #9ca3af;
  cursor: not-allowed;
}

.returned-btn {
  background-color: #9ca3af;
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

  .add-btn {
    width: 100%;
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

  .verify-btn {
    padding: 0.25rem 0.5rem;
    font-size: 0.813rem;
  }
}
</style>
