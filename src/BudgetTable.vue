<!-- BudgetTable.vue -->
<script setup>
import BudgetSummary from './BudgetSummary.vue'

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})

// Обработка удаления — через $emit в шаблоне (defineEmits не обязателен, но лучше указать)
defineEmits(['delete'])

// Вычисляемое свойство для отсортированного списка (без мутации оригинала!)
import { computed } from 'vue'
const sortedTransactions = computed(() => {
  return [...props.transactions].sort((a, b) => b.amount - a.amount)
})

const sortByAmount = () => {
  // Теперь сортируем не напрямую, а через computed — ничего не мутируем!
  // Фактически, переключение порядка сортировки можно добавить позже.
  // Пока просто используем sortedTransactions в шаблоне.
}
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4">
    <h2 class="text-lg font-semibold mb-3">Операции</h2>

    <table class="w-full text-left border-collapse">
      <thead>
        <tr class="border-b">
          <th class="p-2">Название</th>
          <th class="p-2">Тип</th>
          <th class="p-2">
            <!-- Кнопка для сортировки (пока только по убыванию) -->
            <button @click="sortByAmount" class="text-left underline">Сумма</button>
          </th>
          <th class="p-2">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="t in sortedTransactions" :key="t.id" class="border-b">
          <td class="p-2">{{ t.name }}</td>
          <td
            class="p-2 font-medium"
            :class="t.type === 'income' ? 'text-green-600' : 'text-red-600'"
          >
            {{ t.type === 'income' ? 'Доход' : 'Расход' }}
          </td>
          <td class="p-2">
            {{ t.type === 'income' ? '+' : '-' }}{{ Math.abs(t.amount).toFixed(2) }}
          </td>
          <td class="p-2 text-sm text-red-500 cursor-pointer" @click="$emit('delete', t.id)">
            Удалить
          </td>
        </tr>
        <tr v-if="transactions.length === 0">
          <td colspan="4" class="p-2 text-center text-gray-500">Нет операций</td>
        </tr>
      </tbody>
    </table>
  </div>

  <BudgetSummary :transactions="transactions" />
</template>
