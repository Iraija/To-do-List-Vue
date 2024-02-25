<template>
    <div
        v-show="modalActive"
    class="absolute w-full h-screen bg-black bg-opacity-30 top-0 left-0 flex justify-center px-8">
        <div
            v-if="modalActive"
        class="p-4 bg-green-50 self-start mt-32 container max-w-screen-md rounded-md shadow-md">
            
            <div class="flex-1 flex justify-end">
                <i @click="$emit('close-modal')" class="fa-solid fa-circle-xmark text-2xl text-green-400 hover:text-green-500 hover:scale-[100.5%] duration-150 cursor-pointer"></i>
            </div>
            <slot />
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    const newTodo = ref('')

    function addTodo() {
        newTodo.value = ''
    }

    defineEmits(['close-modal'])
    defineProps({
        modalActive: {
            type: Boolean,
            default: false,
        },
    });
</script>