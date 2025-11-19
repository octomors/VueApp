<script setup>
import { ref, onMounted, watch } from 'vue'
import BudgetForm from './BudgetForm.vue'
import BudgetTable from './BudgetTable.vue'

// Состояние: список транзакций
const transactions = ref([])

// Загрузка из localStorage при старте
onMounted(() => {
  const saved = localStorage.getItem('transactions')
  if (saved) {
    transactions.value = JSON.parse(saved)
  }
})

// Сохранение в localStorage при изменении
watch(
  transactions,
  (newVal) => {
    localStorage.setItem('transactions', JSON.stringify(newVal))
  },
  { deep: true },
)

// Методы для работы с данными
const addTransaction = (transaction) => {
  transactions.value.push({
    id: Date.now(), // простой уникальный ID
    ...transaction,
  })
}

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id)
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-900 p-6">
    <h1 class="text-3xl font-bold mb-6 text-center">Калькулятор бюджета</h1>

    <div class="max-w-3xl mx-auto space-y-8">
      <BudgetForm @add="addTransaction" />
      <BudgetTable :transactions="transactions" @delete="deleteTransaction" />
    </div>
  </div>
</template>
