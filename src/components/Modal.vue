<script setup>
import { ref } from 'vue'

const emit = defineEmits(['create-task', 'close'])

const newTask = ref('')

const handleCreate = () => {
if (newTask.value.trim()) {
    const task = {
    id: Date.now(),
    title: newTask.value.trim(),
    status: false,
    createdAt: new Date().toISOString()
    }
    emit('create-task', task)
    newTask.value = '' // очистка поля
    emit('close') // закрыть модальное окно
}
}
</script>
<template>
    <div class="modal">
        <div class="modal__content">
        <button class="modal__close" @click="$emit('close')">
            <!-- Иконка крестика -->
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18" />
            <line x1="6" y1="6" x2="18" y2="18" />
            </svg>
        </button>

        <h3 class="modal__title">Создать новую задачу</h3>
        <span>Описание</span>
        <input v-model="newTask" class="modal__input" placeholder="Введите описание" />

        <button class="modal__create" @click="handleCreate">Создать</button>
        </div>
    </div>
</template>

<style scoped lang="scss">
.modal {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(2px);
    display: flex;
    justify-content: center;
    align-items: center;

    &__content {
        background: white;
        padding: 20px;
        border-radius: 8px;
        position: relative;
        width: 300px;
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    }

    &__close {
        position: absolute;
        right: 8px;
        top: 8px;
        background: none;
        border: none;
        cursor: pointer;
    }

    &__title {
        margin-bottom: 16px;
    }

    &__input {
        width: 100%;
        padding: 8px;
        margin-bottom: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    &__create {
        background: #f0f5ff;
        border: none;
        color: #314b99;
        padding: 8px 16px;
        border-radius: 4px;
        cursor: pointer;

        &:hover {
        border: 1px solid #314b99;
        }
    }
}
</style>