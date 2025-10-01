<script lang="ts" setup>
const currentYear = new Date().getFullYear();
</script>

<template>
    <div class="flex justify-center items-center p-4">
        <p class="text-base font-normal">&copy;{{ currentYear }}. Tools</p>
    </div>
</template>