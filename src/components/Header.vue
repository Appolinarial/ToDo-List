<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
search: String,
sort: String
})

const emit = defineEmits(['update:search', 'update:sort', 'open-modal'])

const dropdownOpen = ref(false)

const toggleDropdown = () => {
dropdownOpen.value = !dropdownOpen.value
}

const selectSort = (value) => {
emit('update:sort', value)
dropdownOpen.value = false
}

const sortText = computed(() => {
return props.sort === 'status' ? 'Статус' : 'Дата'
})
</script>
<template>
    <div class="todo-header">
        <div class="todo-header__top">
        <h2 class="todo-header__title">To do list</h2>
        <button class="todo-header__add-btn" @click="$emit('open-modal')" aria-label="Добавить задачу">
            <svg xmlns="http://www.w3.org/2000/svg" class="todo-header__icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#314B99" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="12" y1="3" x2="12" y2="21"/>
            <line x1="3" y1="12" x2="21" y2="12"/>
            </svg>
        </button>
        </div>

        <div class="todo-header__controls">
        <input
            :value="props.search"
            @input="$emit('update:search', $event.target.value)"
            class="todo-header__search"
            placeholder="Поиск Имени, статуса или даты"
        />

        <div class="todo-header__sort" @click="toggleDropdown">
            <span class="todo-header__sort-label">Сортировать по: </span>
            <span class="todo-header__sort-selected">
            {{ sortText }}
            <svg xmlns="http://www.w3.org/2000/svg" class="todo-header__sort-icon" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <polyline points="6 9 12 15 18 9" />
            </svg>
            </span>

            <ul v-if="dropdownOpen" class="todo-header__dropdown">
            <li @click.stop="selectSort('date')" :class="{ 'todo-header__dropdown-item--active': props.sort === 'date' }">Дата</li>
            <li @click.stop="selectSort('status')" :class="{ 'todo-header__dropdown-item--active': props.sort === 'status' }">Статус</li>
            </ul>
        </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.todo-header {
    &__top {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    &__title {
        font-weight: bold;
    }

    &__add-btn {
        background-color: #D6DBEB;
        border: none;
        border-radius: 50%;
        padding: 8px;
        cursor: pointer;

        &:hover {
        background-color: #D6DBEB;
        }
    }

    &__icon {
        display: block;
    }

    &__controls {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: none;
    }

    &__search {
        border: none;
    }

    &__sort {
        position: relative;
        display: flex;
        align-items: center;
        user-select: none;
        color: #000;
        cursor: pointer;

        &-label {
        margin-right: 4px;
        }

        &-selected {
        display: flex;
        align-items: center;
        }

        &-icon {
        margin-top: 2px;
        }
    }

    &__dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        margin-top: 4px;
        background-color: #fff;
        border: 1px solid #ddd;
        border-radius: 4px;
        width: 120px;
        z-index: 10;
        list-style: none;
        padding: 0;

        li {
        padding: 8px;
        cursor: pointer;

        &:hover {
            background-color: #F6F9FF;
        }

        &.todo-header__dropdown-item--active {
            font-weight: bold;
        }
        }
    }
}
</style>  