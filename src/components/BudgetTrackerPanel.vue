<script setup>
import { reactive, computed } from 'vue';
import { Icon } from '@iconify/vue';
import MonthSelection from './MonthSelection.vue';

// create a budgetTrackerState. This will hold all the data income, expenses, savings, etc. for each month. This will also hold which is the current selected month. Application will default to the current month.

const budgetTrackerState = reactive({
    currentMonth: 8,
    currentYear: 2023,
    // next we need store data for income, expenses and savings for each month/year combination. Multi-dimensional array?
    // we need to be able to easily access the data for the current month. We can do this with a computed property
    // first create the empty data structure
    income: [],
    expenses: [],
    savings: [],
});

// now create the computed property
const currentMonthData = computed(() => {
    // first we need to find the index of the current month/year combination
    const index = budgetTrackerState.income.findIndex(
        (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
    );
    // now we need to return the data for that month
    return {
        income: budgetTrackerState.income[index],
        expenses: budgetTrackerState.expenses[index],
        savings: budgetTrackerState.savings[index]
    };
});

const decreaseMonth = () => {
    if (budgetTrackerState.currentMonth == 1) {
        budgetTrackerState.currentMonth = 12;
        budgetTrackerState.currentYear--;
    } else {
        budgetTrackerState.currentMonth--;
    }
};

const increaseMonth = () => {
    if (budgetTrackerState.currentMonth == 12) {
        budgetTrackerState.currentMonth = 1;
        budgetTrackerState.currentYear++;
    } else {
        budgetTrackerState.currentMonth++;
    }
};
</script>

<template>
    <div class="w-full text-center my-4">
        <!-- This is the main component for budget tracking. All data stored in this component -->
        <!-- 1. Will need to be able to display the current month -->
        <!-- 2. Will need to be able to flick back and forth between months -->
        <!-- 3. Will have a section containing 3 columns (components) income, expenses, savings. Totals for each will be at the top of the columns -->
        <!-- 4. Below these three financial columns will be the summary for the month -->
        <!-- 5. Below the summary for the month will be a yearly summary -->

        <MonthSelection :currentMonth="budgetTrackerState.currentMonth" :currentYear="budgetTrackerState.currentYear"
            :decreaseMonth="decreaseMonth" :increaseMonth="increaseMonth" />

        
    </div>
</template>

<style scoped></style>