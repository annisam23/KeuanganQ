<template>
  <div class="transaction-list">
    <div v-if="transactions.length > 0">
      <div 
        v-for="transaction in transactions" 
        :key="transaction.id"
        class="transaction-item"
      >
        <div class="transaction-info">
          <div class="transaction-category">
            {{ transaction.category }}
          </div>
          <div v-if="transaction.note" class="transaction-note">
            {{ transaction.note }}
          </div>
          <div class="transaction-date">
            <span>📅</span>
            {{ formatDate(transaction.date) }}
          </div>
        </div>
        <div class="transaction-actions">
          <div 
            class="transaction-amount" 
            :class="transaction.type"
          >
            {{ transaction.type === 'income' ? '+' : '-' }}{{ formatCurrency(transaction.amount) }}
          </div>
          <button 
            class="btn-delete" 
            @click="handleDelete(transaction.id)"
          >
            🗑️ Hapus
          </button>
        </div>
      </div>
    </div>
    <div v-else class="empty-state">
      <div class="empty-state-icon">📭</div>
      <p class="empty-state-text">Belum ada transaksi</p>
      <p class="empty-state-subtext">Mulai catat pemasukan dan pengeluaran Anda</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TransactionList',
  props: {
    transactions: {
      type: Array,
      required: true
    }
  },
  emits: ['delete-transaction'],
  methods: {
    formatCurrency(amount) {
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
        minimumFractionDigits: 0
      }).format(amount)
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString('id-ID', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      })
    },
    handleDelete(id) {
      if (confirm('Yakin ingin menghapus transaksi ini?')) {
        this.$emit('delete-transaction', id)
      }
    }
  }
}
</script>

<style scoped>
.transaction-list {
  max-height: 500px;
  overflow-y: auto;
  margin-top: 20px;
}

.transaction-list::-webkit-scrollbar {
  width: 8px;
}

.transaction-list::-webkit-scrollbar-track {
  background: #f3f4f6;
  border-radius: 10px;
}

.transaction-list::-webkit-scrollbar-thumb {
  background: #d1d5db;
  border-radius: 10px;
}

.transaction-list::-webkit-scrollbar-thumb:hover {
  background: #9ca3af;
}

.transaction-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  border-bottom: 1px solid #e5e7eb;
  transition: all 0.3s;
  gap: 16px;
}

.transaction-item:hover {
  background: #f9fafb;
  padding-left: 24px;
}

.transaction-item:last-child {
  border-bottom: none;
}

.transaction-info {
  flex: 1;
  min-width: 0;
}

.transaction-category {
  font-weight: 600;
  color: #1f2937;
  margin-bottom: 6px;
  font-size: 1.05em;
}

.transaction-note {
  color: #6b7280;
  font-size: 0.9em;
  margin-top: 4px;
  font-style: italic;
}

.transaction-date {
  font-size: 0.85em;
  color: #6b7280;
  display: flex;
  align-items: center;
  gap: 4px;
}

.transaction-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.transaction-amount {
  font-size: 1.3em;
  font-weight: 700;
  white-space: nowrap;
}

.transaction-amount.income {
  color: #10b981;
}

.transaction-amount.expense {
  color: #ef4444;
}

.btn-delete {
  padding: 8px 16px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.85em;
  font-weight: 500;
  transition: all 0.3s;
  white-space: nowrap;
}

.btn-delete:hover {
  background: #dc2626;
  transform: scale(1.05);
}

.empty-state {
  text-align: center;
  padding: 60px 20px;
}

.empty-state-icon {
  font-size: 4em;
  margin-bottom: 16px;
  opacity: 0.5;
}

.empty-state-text {
  font-size: 1.1em;
  margin-bottom: 8px;
  color: #6b7280;
}

.empty-state-subtext {
  font-size: 0.9em;
  color: #9ca3af;
}

@media (max-width: 640px) {
  .transaction-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }

  .transaction-actions {
    width: 100%;
    justify-content: space-between;
  }
}
</style>
