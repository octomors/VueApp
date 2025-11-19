<!-- BudgetForm.vue -->
<script setup>
import { ref } from 'vue'

const name = ref('')
const amount = ref('')
const type = ref('income')

const emit = defineEmits(['add'])

const handleSubmit = () => {
  if (!name.value || !amount.value) return

  emit('add', {
    name: name.value.trim(),
    amount: parseFloat(amount.value),
    type: type.value,
  })

  // Очистка формы
  name.value = ''
  amount.value = ''
  type.value = 'income'
}
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4 flex flex-row gap-4">
    <input v-model="name" type="text" placeholder="Название" class="border p-2 flex-1" />
    <input
      v-model.number="amount"
      type="number"
      placeholder="Сумма"
      class="border p-2 w-32"
      min="0"
    />
    <select v-model="type" class="border p-2 w-32">
      <option value="income">Доход</option>
      <option value="expense">Расход</option>
    </select>
    <button
      type="button"
      @click="handleSubmit"
      class="bg-blue-600 text-white px-4 py-2 hover:bg-blue-700 transition"
    >
      Добавить
    </button>
  </div>
</template>
