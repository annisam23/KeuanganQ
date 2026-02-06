<template>
  <form @submit.prevent>
    <div class="form-group">
      <label class="form-label">Kategori</label>
      <select 
        class="form-select"
        v-model="form.category" 
        required
      >
        <option value="" disabled>Pilih kategori...</option>
        <optgroup label="Pemasukan">
          <option value="Gaji">Gaji</option>
          <option value="Bonus">Bonus</option>
          <option value="Freelance">Freelance</option>
          <option value="Investasi">Investasi</option>
          <option value="Hadiah">Hadiah</option>
          <option value="Pemasukan Lainnya">Pemasukan Lainnya</option>
        </optgroup>
        <optgroup label="Pengeluaran">
          <option value="Makan & Minum">Makan & Minum</option>
          <option value="Transport">Transport</option>
          <option value="Belanja">Belanja</option>
          <option value="Tagihan">Tagihan (Listrik, Air, Internet)</option>
          <option value="Hiburan">Hiburan</option>
          <option value="Kesehatan">Kesehatan</option>
          <option value="Pendidikan">Pendidikan</option>
          <option value="Investasi">Investasi</option>
          <option value="Donasi">Donasi</option>
          <option value="Pengeluaran Lainnya">Pengeluaran Lainnya</option>
        </optgroup>
      </select>
    </div>
    <div class="form-group">
      <label class="form-label">Jumlah (Rp)</label>
      <input 
        type="text" 
        class="form-input"
        :value="displayAmount" 
        @input="handleAmountInput"
        placeholder="0"
        required
      >
    </div>
    <div class="form-group">
      <label class="form-label">Tanggal</label>
      <input 
        type="date" 
        class="form-input"
        v-model="form.date"
        required
      >
    </div>
    <div class="form-group">
      <label class="form-label">Catatan (Opsional)</label>
      <input 
        type="text" 
        class="form-input"
        v-model="form.note" 
        placeholder="Tambahkan catatan..."
      >
    </div>
    <div class="btn-group">
      <button type="button" class="btn btn-income" @click="addTransaction('income')">
        <span>➕</span> Pemasukan
      </button>
      <button type="button" class="btn btn-expense" @click="addTransaction('expense')">
        <span>➖</span> Pengeluaran
      </button>
    </div>
  </form>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'TransactionForm',
  emits: ['add-transaction'],
  setup(props, { emit }) {
    const form = ref({
      category: '',
      amount: '',
      date: new Date().toISOString().split('T')[0],
      note: ''
    })

    const displayAmount = ref('')

    const formatNumber = (value) => {
      // Remove non-digit characters
      const num = value.replace(/\D/g, '')
      // Add thousand separator
      return num.replace(/\B(?=(\d{3})+(?!\d))/g, '.')
    }

    const handleAmountInput = (event) => {
      const input = event.target.value
      const numericValue = input.replace(/\D/g, '')
      
      // Update actual value (without separator)
      form.value.amount = numericValue
      
      // Update display value (with separator)
      displayAmount.value = formatNumber(input)
    }

    const addTransaction = (type) => {
      if (!form.value.category || !form.value.amount) {
        alert('Mohon isi kategori dan jumlah!')
        return
      }

      emit('add-transaction', {
        type,
        ...form.value,
        amount: parseFloat(form.value.amount)
      })

      // Reset form
      form.value = {
        category: '',
        amount: '',
        date: new Date().toISOString().split('T')[0],
        note: ''
      }
      displayAmount.value = ''
    }

    return { 
      form, 
      displayAmount,
      handleAmountInput,
      addTransaction 
    }
  }
}
</script>

<style scoped>
.form-group {
  margin-bottom: 20px;
}

.form-label {
  display: block;
  margin-bottom: 8px;
  color: #374151;
  font-weight: 600;
  font-size: 0.9em;
}

.form-input,
.form-select {
  width: 100%;
  padding: 14px 16px;
  border: 2px solid #e5e7eb;
  border-radius: 12px;
  font-size: 1em;
  transition: all 0.3s;
  font-family: inherit;
  background-color: white;
}

.form-input:focus,
.form-select:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-select {
  cursor: pointer;
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%23374151' d='M6 9L1 4h10z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 16px center;
  padding-right: 40px;
}

.form-select optgroup {
  font-weight: bold;
  color: #374151;
}

.form-select option {
  padding: 8px;
  font-weight: normal;
}

.form-input::placeholder {
  color: #9ca3af;
}

.btn-group {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.btn {
  padding: 14px 24px;
  border: none;
  border-radius: 12px;
  font-size: 1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  font-family: inherit;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.btn:active {
  transform: scale(0.98);
}

.btn-income {
  background: #10b981;
  color: white;
}

.btn-income:hover {
  background: #059669;
  box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
}

.btn-expense {
  background: #ef4444;
  color: white;
}

.btn-expense:hover {
  background: #dc2626;
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
}

@media (max-width: 640px) {
  .btn-group {
    grid-template-columns: 1fr;
  }
}
</style>