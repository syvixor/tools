<script lang="ts" setup>
</script>

<template>
    <UHeader :toggle="false">
        <template #title>
            <UIcon name="i-lucide-sparkles" class="w-6 h-6 transition-colors hover:bg-primary" />
        </template>
        <template #right>
            <UColorModeButton />
            <UTooltip text="GitHub">
                <UButton to="https://github.com/syvixor/tools" target="_blank" icon="i-lucide-github"
                    aria-label="GitHub" color="neutral" variant="ghost" />
            </UTooltip>
        </template>
    </UHeader>
</template>