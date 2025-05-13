<script setup>
defineProps({
tasks: {
    type: Array,
    required: true
}
})

const emit = defineEmits(['toggle-status'])

function formatDate(dateStr) {
const date = new Date(dateStr)
if (isNaN(date)) return 'Некорректная дата'
return date.toLocaleDateString('ru-RU', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
})
}
</script>
<template>
<table class="task-table">
    <thead>
    <tr class="task-table__header">
        <th class="task-table__cell task-table__cell--icon"></th>
        <th class="task-table__cell">Описание</th>
        <th class="task-table__cell">Статус</th>
        <th class="task-table__cell">Дата</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="task in tasks" :key="task.id" class="task-table__row">
        <td class="task-table__cell task-table__cell--icon">
        <button class="task-table__status-btn" @click="$emit('toggle-status', task.id)">
            <svg v-if="!task.status" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#6c757d" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10" />
            </svg>
            <svg v-else width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#134EC1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10" />
            <polyline points="9 12 12 15 16 10" />
            </svg>
        </button>
        </td>
        <td class="task-table__cell">{{ task.title }}</td>
        <td class="task-table__cell">{{ task.status ? 'Выполнено' : 'Не выполнено' }}</td>
        <td class="task-table__cell">{{ formatDate(task.createdAt) }}</td>
    </tr>
    </tbody>
</table>
</template>
<style lang="scss" scoped>
.task-table {
    width: 100%;
    border-collapse: collapse;

    &__header {
        text-align: left;
        border-bottom: 1px solid #ccc;
    }

    &__row {
        border-bottom: 1px solid #eee;
    }

    &__cell {
        padding: 12px;

        &--icon {
        width: 40px;
        }
    }

    &__status-btn {
        background: none;
        border: none;
        cursor: pointer;
        padding: 0;

        svg {
        display: block;
        }
    }
}
</style>