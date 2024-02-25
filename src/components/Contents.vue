<template>
    <body class="flex-1 flex flex-col container font-Poppins text-xs">
        <div class="flex flex-col sm:flex-row gap-2 mt-2 mb-5 text-green-500">
            <div class="flex gap-1 items-center flex-1 bg-white shadow-md rounded-md font-semibold">
                <div class="flex items-center gap-1 pl-5 pr-2 py-2">
                    <p>Filter</p>
                    <i class="fa-solid fa-sliders"></i>
                </div>
                <select name="filter" id="filter" v-model="filter" class="flex-1 mr-5 px-2 py-1 border-l-2 bg-white border-green-500 cursor-pointer">
                    <option value="1">Display: All to-do list</option>
                    <option value="2">Display: Undone to-do list</option>
                    <option value="3">Display: Done to-do list</option>
                </select>
            </div>
            <div
                @click="toggleModal(1)"
                class="flex gap-1 items-center justify-center flex-1 bg-white shadow-md px-5 py-2 rounded-md font-semibold hover:bg-green-400 hover:text-white duration-150 cursor-pointer"
            >
                <p>Add to-do</p>
                <i class="fa-regular fa-square-plus"></i>
            </div>
        </div>

        <div class="flex-1 flex flex-col mb-5">
            <ul class="flex flex-col gap-2">
                <li v-for="todoItem in filteredTodoItems" :key="todoItem.id" class="w-full px-5 py-2 flex flex-row gap-2 items-center bg-white shadow-xm text-xm rounded-full">
                    <input type="checkbox" v-model="todoItem.done">
                    <div class="flex-1 text-black">
                        <span :class="{ done: todoItem.done }">{{ todoItem.todoItemText }}</span>
                    </div>
                    <div class="flex gap-3">
                        <button @click="editTodoItem(todoItem)"><i class="fa-solid fa-pen-to-square text-green-600 cursor-pointer"></i></button>
                        <button @click="removeTodoItem(todoItem)"><i class="fa-solid fa-trash text-green-600 cursor-pointer"></i></button>
                    </div>
                </li>
            </ul>
        </div>

        <BaseModal :modalState="modalState" @close-modal="toggleModal">
            <form v-if="activeModal" @submit.prevent="addTodoItem" class="flex flex-col gap-2">
                <div>
                    <h1 class="font-bold text-black">Create new to-do:</h1>
                </div>
                <div class="w-full">
                    <input v-model="newTodoItem" placeholder="Input a to-do item" class="text-black w-full p-2 rounded-md border-2 border-green-100">
                </div>
                <div>            
                    <button
                        @click="$emit('close-modal')"
                        class="text-white font-semibold bg-green-400 py-2 px-6 rounded-md shadow-sm hover:bg-green-500 hover:shadow-md hover:scale-[100.5%] duration-150 cursor-pointer"
                    >Save</button>
                </div>
            </form>
            <form v-else-if="!activeModal && selectedTodoItem" @submit.prevent="saveEditedTodoItem" class="flex flex-col gap-2">
                <div>
                    <h1 class="font-bold text-black">Edit to-do item:</h1>
                </div>
                <div class="w-full">
                    <input v-model="selectedTodoItem.todoItemText" placeholder="Input a to-do item" class="text-black w-full p-2 rounded-md border-2 border-green-100">
                </div>
                <div>            
                    <button
                        @click="$emit('close-modal')"
                        class="text-white font-semibold bg-green-400 py-2 px-6 rounded-md shadow-sm hover:bg-green-500 hover:shadow-md hover:scale-[100.5%] duration-150 cursor-pointer"
                    >Save</button>
                </div>
            </form>
        </BaseModal>

    </body>
</template>

<script setup>
    import { computed, ref } from 'vue'
    import BaseModal from './modal/BaseModal.vue'

    const modalState = ref(null)
    const activeModal = ref(true)

    const toggleModal = (active) => {
        modalState.value =!modalState.value;
        if (active === 1){
            activeModal.value = true;
        }else if(active === 2){
            activeModal.value = false;
        }
    }

    let id = 0

    const newTodoItem = ref('')
    const todoItems = ref([])
    const filter = ref('1')
    const selectedTodoItem = ref(null)

    const filteredTodoItems = computed(() => {
        if (filter.value === '1') {
            return todoItems.value
        } else if (filter.value === '2') {
            return todoItems.value.filter((t) => !t.done)
        } else if (filter.value === '3') {
            return todoItems.value.filter((t) => t.done)
        }
    })

    function addTodoItem() {
        if(newTodoItem.value !== ''){
            todoItems.value.push({ id: id++, todoItemText: newTodoItem.value, done: false })
            newTodoItem.value = ''
            modalState.value =!modalState.value;
        }
    }

    function editTodoItem(todoItem) {
        selectedTodoItem.value = todoItem
        toggleModal(2)
    }

    function saveEditedTodoItem() {
        if(selectedTodoItem.value){
            const index = todoItems.value.findIndex((t) => t.id === selectedTodoItem.value.id)
            if(index !== -1) {
                todoItems.value[index].todoItemText = selectedTodoItem.value.todoItemText
                selectedTodoItem.value = null
                modalState.value =!modalState.value
            }
        }
    }

    function removeTodoItem(todoItem) {
        todoItems.value = todoItems.value.filter((t) => t !== todoItem)
    }

</script>

<style>
    .done {
        color: #d3d3d3;
    }
</style>