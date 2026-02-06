<template>
  <div id="app">
    <!-- Header -->
    <header class="app-header">
      <h1>💰 KeuanganQ</h1>
      <p>Kelola Keuangan Pribadi kamu dengan Lebih Baik</p>
    </header>

    <!-- Summary Cards -->
    <div class="summary-grid">
      <SummaryCard
        title="Total Pemasukan"
        :amount="totalIncome"
        type="income"
        subtitle="Uang masuk"
      />
      <SummaryCard
        title="Total Pengeluaran"
        :amount="totalExpense"
        type="expense"
        subtitle="Uang keluar"
      />
      <SummaryCard
        title="Saldo"
        :amount="balance"
        type="balance"
        :subtitle="balance >= 0 ? 'Keuangan sehat' : 'Perlu perhatian'"
      />
    </div>

    <!-- Main Content -->
    <div class="main-layout">
      <!-- Add Transaction Form -->
      <div class="card">
        <div class="card-header">
          <h2 class="card-title">
            <span>📝</span>
            Tambah Transaksi
          </h2>
        </div>
        <TransactionForm @add-transaction="addTransaction" />
      </div>

      <!-- Category Summary -->
      <div class="card">
        <div class="card-header">
          <h2 class="card-title">
            <span>📊</span>
            Ringkasan Kategori
          </h2>
        </div>
        <CategorySummary :expenses-by-category="expensesByCategory" />
      </div>
    </div>

    <!-- Transaction List -->
    <div class="main-layout">
      <div class="card full-width">
        <div class="card-header">
          <div class="transactions-header">
            <h2 class="card-title">
              <span>📋</span>
              Riwayat Transaksi
            </h2>
            <TransactionFilter v-model="filter" />
          </div>
        </div>
        <TransactionList 
          :transactions="filteredTransactions"
          @delete-transaction="deleteTransaction"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import SummaryCard from './components/SummaryCard.vue'
import TransactionForm from './components/TransactionForm.vue'
import CategorySummary from './components/Categorysummary.vue'
import TransactionFilter from './components/TransactionFilter.vue'
import TransactionList from './components/TransactionList.vue'

export default {
  name: 'App',
  components: {
    SummaryCard,
    TransactionForm,
    CategorySummary,
    TransactionFilter,
    TransactionList
  },
  setup() {
    const transactions = ref([])
    const filter = ref('all')

    // Computed Properties
    const totalIncome = computed(() => {
      return transactions.value
        .filter(t => t.type === 'income')
        .reduce((sum, t) => sum + t.amount, 0)
    })

    const totalExpense = computed(() => {
      return transactions.value
        .filter(t => t.type === 'expense')
        .reduce((sum, t) => sum + t.amount, 0)
    })

    const balance = computed(() => {
      return totalIncome.value - totalExpense.value
    })

    const filteredTransactions = computed(() => {
      const filtered = filter.value === 'all' 
        ? transactions.value 
        : transactions.value.filter(t => t.type === filter.value)
      
      return filtered.sort((a, b) => new Date(b.date) - new Date(a.date))
    })

    const expensesByCategory = computed(() => {
      const categories = {}
      transactions.value
        .filter(t => t.type === 'expense')
        .forEach(t => {
          categories[t.category] = (categories[t.category] || 0) + t.amount
        })
      
      // Sort by amount descending
      return Object.fromEntries(
        Object.entries(categories).sort(([,a], [,b]) => b - a)
      )
    })

    // Methods
    const addTransaction = (data) => {
      transactions.value.push({
        id: Date.now(),
        ...data
      })
      saveToLocalStorage()
    }

    const deleteTransaction = (id) => {
      transactions.value = transactions.value.filter(t => t.id !== id)
      saveToLocalStorage()
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('transactions', JSON.stringify(transactions.value))
    }

    const loadFromLocalStorage = () => {
      const saved = localStorage.getItem('transactions')
      if (saved) {
        transactions.value = JSON.parse(saved)
      }
    }

    // Lifecycle
    onMounted(() => {
      loadFromLocalStorage()
    })

    return {
      transactions,
      filter,
      totalIncome,
      totalExpense,
      balance,
      filteredTransactions,
      expensesByCategory,
      addTransaction,
      deleteTransaction
    }
  }
}
</script>

<style>
/* Header Styles */
.app-header {
  text-align: center;
  color: white;
  margin-bottom: 40px;
  animation: fadeInDown 0.6s ease;
}

.app-header h1 {
  font-size: 3em;
  margin-bottom: 10px;
  font-weight: 700;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.app-header p {
  font-size: 1.2em;
  opacity: 0.95;
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  margin-bottom: 40px;
  animation: fadeInUp 0.6s ease 0.1s both;
}

.main-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  margin-bottom: 24px;
  animation: fadeInUp 0.6s ease 0.2s both;
}

@media (max-width: 968px) {
  .main-layout {
    grid-template-columns: 1fr;
  }
}

.card {
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.card-header {
  margin-bottom: 24px;
  padding-bottom: 16px;
  border-bottom: 2px solid #f3f4f6;
}

.card-title {
  font-size: 1.5em;
  font-weight: 700;
  color: #1f2937;
  display: flex;
  align-items: center;
  gap: 10px;
}

.full-width {
  grid-column: 1 / -1;
  animation: fadeInUp 0.6s ease 0.3s both;
}

.transactions-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 16px;
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 640px) {
  .app-header h1 {
    font-size: 2em;
  }

  .transactions-header {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>