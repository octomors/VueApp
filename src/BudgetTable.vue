<!-- BudgetTable.vue -->
<script setup>
import { ref, computed } from 'vue'
import BudgetSummary from './BudgetSummary.vue'

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})

defineEmits(['delete'])

// Направление сортировки: true — по убыванию (от большего к меньшему)
const sortDesc = ref(true)

// Переключение направления
const toggleSort = () => {
  sortDesc.value = !sortDesc.value
}

// Отсортированный массив (без мутации props!)
const sortedTransactions = computed(() => {
  const sorted = [...props.transactions]
  if (sortDesc.value) {
    return sorted.sort((a, b) => b.amount - a.amount) // убывание
  } else {
    return sorted.sort((a, b) => a.amount - b.amount) // возрастание
  }
})
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
            <!-- Кнопка для переключения сортировки -->
            <button @click="toggleSort" class="text-left underline cursor-pointer">
              Сумма {{ sortDesc ? '↓' : '↑' }}
            </button>
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
