<script setup>
    import { ref, computed, onBeforeUnmount, onMounted } from 'vue'

    const props = defineProps({
    search: String,
    sort: String
    })

    const emit = defineEmits(['update:search', 'update:sort', 'open-modal'])

    const dropdownOpen = ref(false)
    const dropdownRef = ref(null)

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

    const handleClickOutside = (event) => {
    if (dropdownRef.value && !dropdownRef.value.contains(event.target)) {
        dropdownOpen.value = false
    }
    }

    onMounted(() => {
    document.addEventListener('click', handleClickOutside)
    })

    onBeforeUnmount(() => {
    document.removeEventListener('click', handleClickOutside)
    })
</script>
<template>
    <div class="todo-header">
        <div class="todo-header__top">
        <h2 class="todo-header__title">To do list</h2>
        <button class="todo-header__add-btn" @click="$emit('open-modal')" aria-label="Добавить задачу">
            <img src="@/assets/icons/plus-button.svg" class="todo-header__icon" alt="plus"/>
        </button>
        </div>

        <div class="todo-header__controls">
            <div class="todo-header__controls--input">
                <img src="@/assets/icons/magnifier.svg">
                <input
                    :value="props.search"
                    @input="$emit('update:search', $event.target.value)"
                    class="todo-header__search"
                    placeholder="Поиск Имени, статуса или даты"
                />
            </div>

        <div class="todo-header__sort" @click="toggleDropdown" ref="dropdownRef">
            <span class="todo-header__sort-label">Сортировать по: </span>
            <span class="todo-header__sort-selected">
            {{ sortText }}
            <img class="todo-header__sort-button" src="@/assets/icons/dropdown-button.svg">
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
        padding-left: 40px;
    }

    &__title {
        font-weight: bold;
        margin-bottom: 34px;
    }

    &__add-btn {
        background-color: #D6DBEB;
        border: none;
        border-radius: 50%;
        padding: 8px;
        margin-bottom: 30px;
        cursor: pointer;

        &:hover {
        background-color: #D6DBEB;
        }
    }

    &__icon {
        display: block;
        width: 20px;
        height: 20px;
    }

    &__controls {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: none;
        margin-bottom: 40px;
        padding-left: 40px;

        &--input {
            display: flex;
            justify-content: center;
            color: #000;
        }

    }

    &__search {
        border: none;
        text-align: center;
        font-size: 14px;
        color: #000;
        width: 238px;
        height: 20px
    }

    &__sort {
        position: relative;
        display: flex;
        align-items: center;
        user-select: none;
        color: #000;
        cursor: pointer;

        &-label {
        margin-right: 15px;
        text-shadow: 3px 3px 6px rgba(0,0,0,0.3);
        }

        &-selected {
        display: flex;
        align-items: center;
        text-shadow: 3px 3px 6px rgba(0,0,0,0.3);
        }

        &-icon {
        margin-top: 2px;
        }

        &-button {
        margin-left: 6px;
        filter: drop-shadow(3px 3px 6px rgb(0, 0, 0.3));
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
        width: 170px;
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
            font-weight: 600;
        }
        }
    }
}
</style>  