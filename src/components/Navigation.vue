<template>
    <header class="sticky top-0 shadow-md min-w-full bg-green-400 font-Poppins text-white font-bold">
        <nav class="container flex items-center flex-row py-3">

            <div class="flex items-center gap-3 text-xl ">
                <p>To-do List</p>
                <i class="fa-solid fa-list-check"></i>
            </div>

            <div class="flex flex-1 justify-end">
                <div @click="toggleModal" class="flex items-center gap-1 text-lg shadow-sm hover:text-green-700 hover:scale-[100.5%] duration-150 cursor-pointer">
                    <i class="fa-solid fa-circle-info"></i>
                </div>
            </div>

            <BaseModal :modalState="modalState" @close-modal="toggleModal">
                <div class="flex flex-col gap-5 text-md font-semibold w-full text-green-400">
                    <div class="flex flex-col items-center">
                        <p>Welcome to</p>
                        <div class="flex items-center gap-3 text-4xl xs:text-6xl text-green-500">
                            <p>To-do List</p>
                            <i class="fa-solid fa-list-check"></i>
                        </div>
                        <p class="text-[0.60rem] sm:text-xs text-center "><span class="font-bold text-green-500">MAKE</span> your task, <span class="font-bold text-green-500">DO</span> your task, <span class="font-bold text-green-500">COMPLETE</span> your task</p>
                    </div>
                    <div class="flex flex-col gap-2 px-5 font-normal text-[0.60rem] text-green-500">
                        <p class="font-bold">How to use:</p>
                        <p>1. Click <span class="font-semibold">Add to-do <i class="fa-regular fa-square-plus"></i></span> to add/create a to-do item, then input your task and click save.</p>
                        <p>2. Click the checkbox <span class="font-semibold"><i class="fa-solid fa-square-check"></i></span> when you're to-do item is done.</p>
                        <p>3. Click the edit icon <span class="font-semibold"><i class="fa-solid fa-pen-to-square"></i></span> to edit your to-do item, input your new task and click save.</p>
                        <p>4. Click the delete icon <span class="font-semibold"><i class="fa-solid fa-trash"></i></span> when you want to delete a to-do item.</p>
                        <p>5. Filter your list by clicking <span class="font-semibold">Filter <i class="fa-solid fa-sliders"></i></span> and select your filter option.</p>
                        <p>6. Now <span class="font-bold text-green-500">MAKE</span>, <span class="font-bold text-green-500">DO</span>, and <span class="font-bold text-green-500">COMPLETE</span> your task.</p>
                    </div>
                    
                    <div>            
                        <button
                            @click="toggleModal"
                            class=" text-sm text-white font-semibold bg-green-400 py-2 px-6 rounded-md shadow-sm hover:bg-green-500 hover:shadow-md hover:scale-[100.5%] duration-150 cursor-pointer"
                        >Close</button>
                    </div>
                </div>
            </BaseModal>
            
        </nav>
    </header>
</template>

<script setup>
    import { ref } from 'vue';
    import BaseModal from './modal/BaseModal.vue';

    const modalState = ref(null);
    const toggleModal = () => {
        modalState.value =!modalState.value;
    }

</script>