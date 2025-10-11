<script lang="ts" setup>
const currentYear = new Date().getFullYear();
</script>

<template>
    <UFooter>
        <template #left>
            <p class="text-sm font-normal text-muted">
                Copyright © {{ currentYear }}
            </p>
        </template>
    </UFooter>
</template>