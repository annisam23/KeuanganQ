<template>
  <div class="filter-tabs">
    <button 
      class="filter-tab" 
      :class="{ active: modelValue === 'all' }"
      @click="$emit('update:modelValue', 'all')"
    >
      Semua
    </button>
    <button 
      class="filter-tab" 
      :class="{ active: modelValue === 'income' }"
      @click="$emit('update:modelValue', 'income')"
    >
      Pemasukan
    </button>
    <button 
      class="filter-tab" 
      :class="{ active: modelValue === 'expense' }"
      @click="$emit('update:modelValue', 'expense')"
    >
      Pengeluaran
    </button>
  </div>
</template>

<script>
export default {
  name: 'TransactionFilter',
  props: {
    modelValue: {
      type: String,
      required: true
    }
  },
  emits: ['update:modelValue']
}
</script>

<style scoped>
.filter-tabs {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.filter-tab {
  padding: 10px 20px;
  border: 2px solid #e5e7eb;
  background: white;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 600;
  font-size: 0.9em;
  color: #374151;
}

.filter-tab:hover {
  border-color: #d1d5db;
  background: #f9fafb;
}

.filter-tab.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}
</style>