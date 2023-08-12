<script setup>
// You can import and setup any required Vue or external methods or data here.
</script>

<template>
    <header class="flex justify-between items-center p-4 bg-gray-200 shadow-md">
        <h1 class="text-3xl font-bold text-gray-800 cursor-pointer">
            <router-link to="/" class="hover:text-gray-600 transition">Vue Budget Tracker</router-link>
        </h1>
        <nav class="flex items-center space-x-8">
            <router-link to="/" class="text-gray-800 text-xl hover:text-gray-600 transition" active-class="font-semibold">
                Home
            </router-link>
            <router-link to="/about" class="text-gray-800 text-xl hover:text-gray-600 transition"
                active-class="font-semibold">
                About
            </router-link>
        </nav>
    </header>
</template>

<style scoped>
/* If you'd like to add or modify specific styles for this component, you can do so here. */
</style>
