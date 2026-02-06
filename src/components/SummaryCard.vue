<template>
  <div class="summary-card" :class="type">
    <h3 class="summary-card-title">{{ title }}</h3>
    <div class="summary-card-amount">{{ formatCurrency(amount) }}</div>
    <p class="summary-card-subtitle">{{ subtitle }}</p>
  </div>
</template>

<script>
export default {
  name: 'SummaryCard',
  props: {
    title: {
      type: String,
      required: true
    },
    amount: {
      type: Number,
      required: true
    },
    type: {
      type: String,
      required: true,
      validator: (value) => ['income', 'expense', 'balance'].includes(value)
    },
    subtitle: {
      type: String,
      default: ''
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
.summary-card {
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s, box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}

.summary-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #667eea, #764ba2);
}

.summary-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
}

.summary-card.income::before {
  background: linear-gradient(90deg, #10b981, #059669);
}

.summary-card.expense::before {
  background: linear-gradient(90deg, #ef4444, #dc2626);
}

.summary-card-title {
  color: #6b7280;
  font-size: 0.875em;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 12px;
  font-weight: 600;
}

.summary-card-amount {
  font-size: 2.5em;
  font-weight: 700;
  margin-bottom: 8px;
  line-height: 1;
}

.summary-card.income .summary-card-amount {
  color: #10b981;
}

.summary-card.expense .summary-card-amount {
  color: #ef4444;
}

.summary-card.balance .summary-card-amount {
  color: #667eea;
}

.summary-card-subtitle {
  font-size: 0.85em;
  color: #6b7280;
}
</style>
