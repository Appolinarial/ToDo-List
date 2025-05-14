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
        newTask.value = ''
        emit('close')
    }
}
</script>
<template>
    <div class="modal">
        <div class="modal__content">
        <button class="modal__close" @click="$emit('close')">
            <img src="@/assets/icons/close-button.svg">
        </button>

        <h3 class="modal__title">Создать новую задачу</h3>
        <span>Описание</span>
        <input v-model="newTask" class="modal__input" placeholder="Введите описание" />
        <div class="modal__create">
            <button class="modal__create-button" @click="handleCreate">Создать</button>
        </div>
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
        box-sizing: border-box;
        padding: 40px;
        border-radius: 6px;
        position: relative;
        width: 400px;
        height: 281px;
        box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    }

    &__close {
        position: absolute;
        right: 40px;
        top: 40px;
        background: none;
        border: none;
        cursor: pointer;
    }

    &__title {
        font-family: 'Montserrat', sans-serif;
        font-size: 16px;
        margin-bottom: 30px;
        margin-top: 0;
    }

    &__input {
        width: 100%;
        box-sizing: border-box;
        padding: 11px 0px 11px 16px;
        font-size: 14px;
        margin: 5px 0 30px 0 ;
        border: 1px solid #ccc;
        border-radius: 8px;
        &:hover {
            border: 1px solid #ccc;
        }
    }

    &__create {
        display: flex;
        justify-content: center;
        width:100%;

        &-button {
        position: absolute;
        background: #f0f5ff;
        border: none;
        color: #314b99;
        padding: 12px 40px;
        width: 153px;
        margin: auto;
        border-radius: 8px;
        font-size: 18px;
        cursor: pointer;
        bottom: 50px;
        }
    }
}
</style>