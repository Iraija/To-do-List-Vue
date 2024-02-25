<template>
    <body class="flex-1 flex flex-col container font-Poppins text-green-500 text-sm">
        <div class="flex flex-col sm:flex-row gap-2 mt-2 mb-5">
            <div class="flex gap-1 items-center flex-1 bg-white shadow-md rounded-md font-semibold">
                <div class="flex items-center gap-1 pl-5 pr-2 py-2">
                    <p>Filter</p>
                    <i class="fa-solid fa-sliders"></i>
                </div>
                <select name="filter" id="filter" v-model="filter" class="flex-1 mr-5 px-2 py-1 border-l-2 border-green-500 cursor-pointer">
                    <option value="1">Display: All to-do list</option>
                    <option value="2">Display: Undone to-do list</option>
                    <option value="3">Display: Done to-do list</option>
                </select>
            </div>
            <div
                @click="toggleModal"
                class="flex gap-1 items-center justify-center flex-1 bg-white shadow-md px-5 py-2 rounded-md font-semibold hover:bg-green-400 hover:text-white duration-150 cursor-pointer"
            >
                <p>Add to-do</p>
                <i class="fa-solid fa-pen-to-square"></i>
            </div>
        </div>

        <div class="flex-1 flex flex-col mb-5">
            <ul class="flex flex-col gap-2">
                <li v-for="todoItem in filteredTodoItems" :key="todoItem.id" class="w-full px-5 py-2 flex flex-row gap-2 items-center bg-white shadow-sm text-sm rounded-full">
                    <input type="checkbox" v-model="todoItem.done">
                    <div class="flex-1 text-black">
                        <span :class="{ done: todoItem.done }">{{ todoItem.todoItemText }}</span>
                    </div>
                    <button @click="removeTodoItem(todoItem)"><i class="fa-solid fa-trash text-green-600 cursor-pointer"></i></button>
                </li>
            </ul>
        </div>

        <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
            <form @submit.prevent="addTodoItem" class="flex flex-col gap-2">
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
        </BaseModal>

    </body>
</template>

<script setup>
    import { computed, ref } from 'vue';
    import BaseModal from './modal/BaseModal.vue';

    const modalActive = ref(null);
    const toggleModal = () => {
        modalActive.value =!modalActive.value;
    }

    let id = 0

    const newTodoItem = ref('')
    const todoItems = ref([])
    const filter = ref('1')

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
            modalActive.value =!modalActive.value;
        }
    }

    function removeTodoItem(todoItem) {
        todoItems.value = todoItems.value.filter((t) => t !== todoItem)
    }

</script>