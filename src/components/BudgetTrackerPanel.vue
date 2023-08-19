<script setup>
import { reactive, computed } from 'vue';
import { Icon } from '@iconify/vue';
import MonthSelection from './MonthSelection.vue';
import TransactionList from './TransactionList.vue';

// create a budgetTrackerState. This will hold all the data income, expenses, savings, etc. for each month. This will also hold which is the current selected month. Application will default to the current month.

const budgetTrackerState = reactive({
    currentMonth: 8,
    currentYear: 2023,
    // next we need store data for income, expenses and savings for each month/year combination. Multi-dimensional array?
    // we need to be able to easily access the data for the current month. We can do this with a computed property
    // first create the data structure
    // fill with example data
    income: [],
    expenses: [],
    savings: []
});

// now create the computed property
const currentMonthData = computed(() => {
    console.log("====== currentMonthData() ======");
    // first we need to find the index of the current month/year combination for each of the income, expenses and savings arrays
    console.log("determining data index for current month/year combination. Month: " + budgetTrackerState.currentMonth + ", Year: " + budgetTrackerState.currentYear + ".");
    const incomeIndex = budgetTrackerState.income.findIndex(
        (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
    );
    const expensesIndex = budgetTrackerState.expenses.findIndex(
        (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
    );
    const savingsIndex = budgetTrackerState.savings.findIndex(
        (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
    );

    console.log("incomeIndex: " + incomeIndex);
    console.log("expensesIndex: " + expensesIndex);
    console.log("savingsIndex: " + savingsIndex);

    console.log("returning data for current month/year combination");
    return {
        income: incomeIndex != -1 ? budgetTrackerState.income[incomeIndex] : null,
        expenses: expensesIndex != -1 ? budgetTrackerState.expenses[expensesIndex] : null,
        savings: savingsIndex != -1 ? budgetTrackerState.savings[savingsIndex] : null
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

const addTransaction = (type, description, amount) => {
    console.log("==== addTransaction() with parameters: " + type + ", " + description + ", " + amount);
    // first we need to find the index of the current month/year combination

    // now we need to determine wether to add to income, expenses or savings
    if (type == 'Income') {
        console.log("adding to income");
        var index = budgetTrackerState.income.findIndex(
            (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
        );
        console.log("index: " + index);

        // if index not found for the month, year combination, create a new entry in the array
        if (index == -1) {
            console.log("index not found, creating new entry");
            budgetTrackerState.income.push({
                month: budgetTrackerState.currentMonth,
                year: budgetTrackerState.currentYear,
                transactions: []
            });
            // update index
            index = budgetTrackerState.income.length - 1;
        }
        console.log("new index: " + index);

        console.log("adding transaction")
        budgetTrackerState.income[index].transactions.push({
            id: budgetTrackerState.income[index].transactions.length + 1,
            description: description,
            amount: amount
        });
    } else if (type == 'Expenses') {
        console.log("adding to expenses");
        var index = budgetTrackerState.expenses.findIndex(
            (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
        );
        console.log("index: " + index);

        // if index not found for the month, year combination, create a new entry in the array
        if (index == -1) {
            console.log("index not found, creating new entry");
            budgetTrackerState.expenses.push({
                month: budgetTrackerState.currentMonth,
                year: budgetTrackerState.currentYear,
                transactions: []
            });
            // update index
            index = budgetTrackerState.expenses.length - 1;
        }
        console.log("new entry: " + budgetTrackerState.expenses.length[index]);

        console.log("adding transaction")
        budgetTrackerState.expenses[index].transactions.push({
            id: budgetTrackerState.expenses[index].transactions.length + 1,
            description: description,
            amount: amount
        });
    } else if (type == 'Savings') {
        console.log("adding to savings");

        var index = budgetTrackerState.savings.findIndex(
            (item) => item.month == budgetTrackerState.currentMonth && item.year == budgetTrackerState.currentYear
        );

        console.log("index: " + index);

        // if index not found for the month, year combination, create a new entry in the array
        if (index == -1) {
            console.log("index not found, creating new entry");
            budgetTrackerState.savings.push({
                month: budgetTrackerState.currentMonth,
                year: budgetTrackerState.currentYear,
                transactions: []
            });
            // update index
            index = budgetTrackerState.savings.length - 1;
        }
        console.log("new index: " + index);

        console.log("adding transaction")
        budgetTrackerState.savings[index].transactions.push({
            id: budgetTrackerState.savings[index].transactions.length + 1,
            description: description,
            amount: amount
        });
    }
};
</script>

<template>
    <div class="w-full my-4">
        <!-- This is the main component for budget tracking. All data stored in this component -->
        <!-- 1. Will need to be able to display the current month -->
        <!-- 2. Will need to be able to flick back and forth between months -->
        <!-- 3. Will have a section containing 3 columns (components) income, expenses, savings. Totals for each will be at the top of the columns -->
        <!-- 4. Below these three financial columns will be the summary for the month -->
        <!-- 5. Below the summary for the month will be a yearly summary -->

        <MonthSelection @decrease-month="decreaseMonth" @increase-month="increaseMonth"
            :currentMonth="budgetTrackerState.currentMonth" :currentYear="budgetTrackerState.currentYear"
            :decreaseMonth="decreaseMonth" :increaseMonth="increaseMonth" />

        <!-- Create 3 columns. One for income, one for expenses and one for savings -->
        <div class="flex flex-col space-y-2 px-4 mt-4 justify-between md:flex-row md:space-x-2 md:space-y-0">
            <TransactionList :type="'Income'" :currentMonthData="currentMonthData.income"
                @add-transaction="addTransaction" />
            <TransactionList :type="'Expenses'" :currentMonthData="currentMonthData.expenses"
                @add-transaction="addTransaction" />
            <TransactionList :type="'Savings'" :currentMonthData="currentMonthData.savings"
                @add-transaction="addTransaction" />
        </div>
    </div>
</template>

<style scoped></style>