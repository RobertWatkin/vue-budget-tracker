<script setup>
import { defineProps, reactive } from 'vue';
import { Icon } from '@iconify/vue';
const props = defineProps({
    type: {
        type: String,
        required: true
    },
    currentMonthData: {
        type: Object,
        required: false
    },
});

// declare emit events
defineEmits(['add-transaction']);

// reactive inputs
const input = reactive({
    description: '',
    amount: 0
});

const clearInput = () => {
    input.description = '';
    input.amount = 0;
};
</script>

<template>
    <div class="md:w-1/3 bg-white rounded-lg">
        <h3 class="text-xl font-bold ml-4">{{ props.type }}</h3>

        <!-- Display list of Income -->
        <table class="min-w-full divide-y divide-gray-200 mb-4">
            <thead class="bg-gray-50">
                <tr>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                        Description
                    </th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                        Amount
                    </th>
                </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
                <tr v-if="props.currentMonthData" v-for="item in props.currentMonthData.transactions" :key="item.id">
                    <td class="px-2 py-4 whitespace-nowrap">
                        <div class="font-bold text-gray-900">{{ item.description }}</div>
                    </td>
                    <td class="px-2 py-4 whitespace-nowrap">
                        <div class="text-gray-500">{{ item.amount }}</div>
                    </td>
                </tr>
                <!-- Input row -->
                <tr>
                    <td class="py-4">
                        <input type="text" placeholder="Description" class="w-full p-2 border-2 rounded" v-model="input.description" />
                    </td>
                    <td class="py-4 flex items-center">
                        <input type="number" step="0.01" min="0" placeholder="Amount" class="flex-1 mr-4 p-2 border-2 rounded" v-model="input.amount" />
                        <Icon icon="ri:add-circle-line" width="22" color="#a2a2a2" class="hover:scale-110 hover:cursor-pointer mr-2" @click="$emit('add-transaction', props.type, input.description, input.amount); clearInput()" />
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style lang="scss" scoped></style>