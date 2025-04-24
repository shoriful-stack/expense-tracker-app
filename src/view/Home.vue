<template>
    <div class="min-h-screen bg-dark text-white d-flex flex-column align-items-center p-5">
        <h2 class="mb-4">Expense Tracker</h2>
        <AddTransactionForm v-model:title="title" v-model:amount="amount" v-model:type="type" @add="addTransaction" />
        <TransactionList :transactions="transactions" :filter="filter" :page="page" :itemsPerPage="itemsPerPage"
            @delete="deleteTransaction" @update:page="page = $event" v-model:filter="filter" />
    </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import AddTransactionForm from '../components/AddTransactionForm.vue'
import TransactionList from '../components/TransactionList.vue'

const title = ref('')
const amount = ref(0)
const type = ref('INCOME')
const filter = ref('all')
const page = ref(1)
const itemsPerPage = 5

const transactions = ref([])

const load = () => {
    const stored = localStorage.getItem('transactions')
    if (stored) transactions.value = JSON.parse(stored)
}
const save = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

const addTransaction = () => {
    if (!title.value || amount.value <= 0 || !type.value) return
    transactions.value.push({
        id: Date.now(),
        title: title.value,
        amount: amount.value,
        type: type.value,
    })
    title.value = ''
    amount.value = 0
    type.value = 'INCOME'
    page.value = 1
}

const deleteTransaction = (id) => {
    transactions.value = transactions.value.filter(tx => tx.id !== id)
}

watch(transactions, save, { deep: true })
onMounted(load)
</script>