<script setup>
import { computed } from 'vue';
import { Icon } from '@iconify/vue';
// import emit

const props = defineProps({
    currentMonth: {
        type: Number,
        required: true
    },
    currentYear: {
        type: Number,
        required: true
    },
    decreaseMonth: {
        type: Function,
        required: true
    },
    increaseMonth: {
        type: Function,
        required: true
    },
});

const currentMonthText = computed(() => {
    const date = new Date(props.currentYear, props.currentMonth - 1);
    return date.toLocaleString('default', { month: 'long', year: 'numeric' });
});

// declare emit events
defineEmits(['decrease-month', 'increase-month']);
</script>

<template>
    <div class="flex flex-between max-w-sm mx-auto">
        <Icon icon="solar:alt-arrow-left-bold" class="hover:cursor-pointer hover:scale-110" width="36"
            @click="$emit('decrease-month')" />
        <h2 class="mx-auto text-2xl font-bold">{{ currentMonthText }}</h2>
        <Icon icon="solar:alt-arrow-right-bold" class="hover:cursor-pointer hover:scale-110" width="36"
            @click="$emit('increase-month')" />
    </div>
</template>

<style lang="scss" scoped></style>