<template>
  <div class="category-list">
    <div v-if="Object.keys(expensesByCategory).length > 0">
      <div 
        v-for="(amount, category) in expensesByCategory" 
        :key="category"
        class="category-item"
      >
        <span class="category-name">{{ category }}</span>
        <span class="category-amount">{{ formatCurrency(amount) }}</span>
      </div>
    </div>
    <div v-else class="empty-state">
      <div class="empty-state-icon">📂</div>
      <p class="empty-state-text">Belum ada kategori</p>
      <p class="empty-state-subtext">Tambahkan pengeluaran pertama Anda</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CategorySummary',
  props: {
    expensesByCategory: {
      type: Object,
      required: true
    }
  },
  methods: {
    formatCurrency(amount) {
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
        minimumFractionDigits: 0
      }).format(amount)
    }
  }
}
</script>

<style scoped>
.category-list {
  max-height: 400px;
  overflow-y: auto;
}

.category-list::-webkit-scrollbar {
  width: 8px;
}

.category-list::-webkit-scrollbar-track {
  background: #f3f4f6;
  border-radius: 10px;
}

.category-list::-webkit-scrollbar-thumb {
  background: #d1d5db;
  border-radius: 10px;
}

.category-list::-webkit-scrollbar-thumb:hover {
  background: #9ca3af;
}

.category-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  border-bottom: 1px solid #e5e7eb;
  transition: background 0.3s;
}

.category-item:hover {
  background: #f9fafb;
}

.category-item:last-child {
  border-bottom: none;
}

.category-name {
  font-weight: 600;
  color: #374151;
}

.category-amount {
  font-size: 1.1em;
  font-weight: 700;
  color: #ef4444;
}

.empty-state {
  text-align: center;
  padding: 60px 20px;
  color: #9ca3af;
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
</style>