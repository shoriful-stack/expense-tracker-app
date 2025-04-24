<template>
    <div>
        <div class="mb-3 d-flex gap-3 justify-content-center">
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" value="all" v-model="modelValue" />
                <label class="form-check-label">All</label>
            </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" value="INCOME" v-model="modelValue" />
                <label class="form-check-label">Income</label>
            </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" value="EXPENSE" v-model="modelValue" />
                <label class="form-check-label">Expense</label>
            </div>
        </div>

        <table class="table table-dark table-bordered w-auto text-center">
            <thead>
                <tr>
                    <th>Title</th>
                    <th>Amount</th>
                    <th>Type</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="tx in paginated" :key="tx.id">
                    <td>{{ tx.title }}</td>
                    <td :class="amountClass(tx)">
                        ${{ tx.amount }}
                    </td>
                    <td>{{ tx.type }}</td>
                    <td>
                        <button class="btn btn-danger" @click="$emit('delete', tx.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <div class="d-flex justify-content-between align-items-center w-96 mt-3">
            <button class="btn btn-secondary" :disabled="page === 1"
                @click="$emit('update:page', page - 1)">Previous</button>
            <span>Page {{ page }} of {{ totalPages }}</span>
            <button class="btn btn-secondary" :disabled="page === totalPages"
                @click="$emit('update:page', page + 1)">Next</button>
        </div>
    </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['transactions', 'filter', 'page', 'itemsPerPage'])
const emit = defineEmits(['update:filter', 'update:page', 'delete'])

const modelValue = computed({
    get: () => props.filter,
    set: (val) => emit('update:filter', val),
})

const filtered = computed(() => {
    if (props.filter === 'all') return props.transactions
    return props.transactions.filter(tx => tx.type === props.filter)
})

const totalPages = computed(() => Math.ceil(filtered.value.length / props.itemsPerPage))

const paginated = computed(() => {
    const start = (props.page - 1) * props.itemsPerPage
    return filtered.value.slice(start, start + props.itemsPerPage)
})

const amountClass = (tx) => {
    if (tx.type === 'INCOME') return 'text-success'
    if (tx.amount >= 500) return 'text-danger fw-bold'
    return 'text-danger'
}
</script>