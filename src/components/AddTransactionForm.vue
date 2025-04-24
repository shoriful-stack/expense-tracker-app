<template>
    <div class="d-flex gap-2 mb-3 flex-wrap justify-content-center">
        <input v-model="localTitle" type="text" placeholder="Title" class="form-control input-sm"
            style="width: 150px;" />
        <input v-model.number="localAmount" type="number" placeholder="0" class="form-control input-sm"
            style="width: 100px;" />
        <select v-model="localType" class="form-select input-sm" style="width: 150px;">
            <option value="INCOME">Income</option>
            <option value="EXPENSE">Expense</option>
        </select>
        <button class="btn btn-primary" @click="emitAdd">Add</button>
    </div>
</template>

<script setup>
import { watch, ref } from 'vue'

const props = defineProps(['title', 'amount', 'type'])
const emit = defineEmits(['update:title', 'update:amount', 'update:type', 'add'])

const localTitle = ref(props.title)
const localAmount = ref(props.amount)
const localType = ref(props.type)

watch(localTitle, (val) => emit('update:title', val))
watch(localAmount, (val) => emit('update:amount', val))
watch(localType, (val) => emit('update:type', val))

const emitAdd = () => emit('add')
</script>