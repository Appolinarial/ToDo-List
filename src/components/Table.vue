<script setup>
import { ref } from 'vue'

const hoveredRow = ref(null)

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
  <div class="task-table">
    <div class="task-table__header task-table__row">
      <div class="task-table__cell task-table__cell--icon"></div>
      <div class="task-table__cell task-table__cell--header">Описание</div>
      <div class="task-table__cell task-table__cell--header task-table__cell--status ">Статус</div>
      <div class="task-table__cell task-table__cell--header task-table__cell--date">Дата</div>
    </div>

    <div
      class="task-table__row"
      v-for="task in tasks"
      :key="task.id"
      @mouseenter="hoveredRow = task.id"
      @mouseleave="hoveredRow = null"
      :class="{ 'is-hovered': hoveredRow === task.id }"
    >
      <div class="task-table__cell task-table__cell--icon">
        <button class="task-table__status-btn" @click="$emit('toggle-status', task.id)">
          <img v-if="!task.status" src="@/assets/icons/circle.svg" />
          <img v-else class="task-table__status-btn-done" src="@/assets/icons/done-circle.svg" />
        </button>
      </div>
      <div class="task-table__cell">{{ task.title }}</div>
      <div
        class="task-table__cell task-table__cell--status"
        :class="task.status ? 'status-done' : 'status-in-progress'"
      >
        {{ task.status ? 'Выполнено' : 'В работе' }}
      </div>
      <div class="task-table__cell task-table__cell--date">{{ formatDate(task.createdAt) }}</div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.task-table {
  display: grid;
  width: 100%;
  font-size: 14px;
  margin-bottom: 16px;

  &__row {
    display: grid;
    grid-template-columns: 80px 1fr 151px 129px;
    height: 58px;
    align-items: center;
    background-color: white;
    transition: box-shadow 0.2s ease, background-color 0.2s ease;
    position: relative;
    z-index: 0;
    margin-bottom: 0;
    border-bottom: 1px solid #eee;

    &.is-hovered {
      background-color: #f6f9ff;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      z-index: 1;
    }

    &:not(.is-hovered) {
      box-shadow: none;
    }
  }

  &__header {
    font-weight: 100;
  }

  &__cell {
    padding: 16px 20px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;

    &--header {
        border-left: 1px solid #eee;
        padding: 10px 0px 6px 20px;
        margin-bottom: 16px;
    }

    &--icon {
      display: flex;
      justify-content: center;
      padding-left: 0;
    }
  }

  &__status-btn {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    overflow: visible;

    &-done {
      filter: drop-shadow(0 4px 3px rgba(19, 78, 193, 0.22));
    }
  }
}

.status-done {
  color: #134ec1;
}
.status-in-progress {
  color: #f89b11;
}
</style>