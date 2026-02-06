# 💰 Aplikasi Keuangan Pribadi

Aplikasi keuangan berbasis Vue 3 dengan Composition API untuk mengelola pemasukan dan pengeluaran pribadi.

## ✨ Fitur

- ✅ Catat pemasukan dan pengeluaran
- ✅ Ringkasan total dan saldo
- ✅ Kategorisasi otomatis
- ✅ Filter transaksi
- ✅ Penyimpanan lokal (LocalStorage)
- ✅ Design responsif
- ✅ Animasi smooth

## 🚀 Cara Menjalankan

### Prerequisites
- Node.js (versi 16 atau lebih baru)
- npm atau yarn

### Instalasi

1. Install dependencies:
```bash
npm install
```

2. Jalankan development server:
```bash
npm run dev
```

3. Buka browser di `http://localhost:3000`

### Build untuk Production

```bash
npm run build
```

File hasil build akan ada di folder `dist/`

## 📁 Struktur Project

```
finance-vue-project/
├── src/
│   ├── components/
│   │   ├── SummaryCard.vue        # Kartu ringkasan keuangan
│   │   ├── TransactionForm.vue    # Form input transaksi
│   │   ├── CategorySummary.vue    # Ringkasan per kategori
│   │   ├── TransactionFilter.vue  # Filter transaksi
│   │   └── TransactionList.vue    # Daftar transaksi
│   ├── App.vue                     # Komponen utama
│   ├── main.js                     # Entry point
│   └── style.css                   # Global styles
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

## 🛠️ Teknologi

- Vue 3 (Composition API)
- Vite (Build tool)
- CSS3 (dengan custom properties)
- LocalStorage API

## 📝 Lisensi

MIT License - Bebas digunakan untuk proyek pribadi atau komersial
