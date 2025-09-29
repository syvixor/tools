<script lang="ts" setup>
import type { TabsItem } from "@nuxt/ui";

const statements = ["Privacy-First", "No Tracking", "Local Processing Only"];

const tools = [
    {
        label: "UUID Generator",
        slot: "generator" as const
    },
    {
        label: "Image Converter",
        slot: "converter" as const
    },
    {
        label: "Encode / Decode",
        slot: "codex" as const
    }
] satisfies TabsItem[];
</script>

<template>
    <div class="space-y-8 p-4">
        <div class="flex flex-col justify-center items-center gap-4">
            <UIcon name="i-lucide-wind" class="w-16 h-16" />
            <p class="text-base text-center font-medium uppercase">{{ statements.join(" • ") }}</p>
        </div>
        <div class="flex flex-col justify-center items-center w-full mx-auto md:w-3/4">
            <UTabs :items="tools" variant="link" :ui="{ trigger: 'grow' }" class="w-full">
                <template #generator>
                    <Generator />
                </template>
                <template #converter>
                    <Converter />
                </template>
                <template #codex>
                    <Codex />
                </template>
            </UTabs>
        </div>
    </div>
</template>