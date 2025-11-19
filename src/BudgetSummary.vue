<!-- BudgetSummary.vue -->
<script setup>
import { computed } from 'vue'

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})

const totalIncome = computed(() =>
  props.transactions.filter((t) => t.type === 'income').reduce((sum, t) => sum + t.amount, 0),
)

const totalExpenses = computed(() =>
  props.transactions.filter((t) => t.type === 'expense').reduce((sum, t) => sum + t.amount, 0),
)

const balance = computed(() => totalIncome.value - totalExpenses.value)
const isNegative = computed(() => balance.value < 0)
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4 text-center mt-6">
    <h2 class="text-lg font-semibold mb-3">Сводка</h2>

    <div class="grid grid-cols-3 gap-4">
      <div>
        <p class="text-gray-500">Доходы</p>
        <p class="text-green-600 text-xl font-semibold">{{ totalIncome.toFixed(2) }}</p>
      </div>
      <div>
        <p class="text-gray-500">Расходы</p>
        <p class="text-red-600 text-xl font-semibold">{{ totalExpenses.toFixed(2) }}</p>
      </div>
      <div>
        <p class="text-gray-500">Баланс</p>
        <p class="text-gray-800 text-xl font-semibold">{{ balance.toFixed(2) }}</p>
      </div>
    </div>

    <p v-if="isNegative" class="mt-4 text-red-500 font-medium">⚠️ Ваш баланс отрицательный!</p>
  </div>
</template>
