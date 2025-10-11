<script lang="ts" setup>
const tools = [
    {
        to: "/uuid-generator",
        icon: "i-lucide-sparkles",
        title: "UUID Generator",
        description: "Generate unique UUIDs instantly."
    },
    {
        to: "/image-converter",
        icon: "i-lucide-image",
        title: "Image Converter",
        description: "Convert images between formats quickly and easily.",
        development: false
    },
    {
        to: "/encoder-decoder",
        icon: "i-lucide-code",
        title: "Encoder/Decoder",
        description: "Encode & Decode text using Base64.",
        development: false
    },
    {
        to: undefined,
        icon: "i-lucide-qr-code",
        title: "QR Code Generator",
        description: "Generate QR codes for links, text, or any data.",
        development: true
    }
];
</script>

<template>
    <UPageGrid class="gap-4 p-4">
        <UPageCard :to="tool.to" :icon="tool.icon" :title="tool.title" spotlight v-for="(tool, index) in tools"
            :key="index" variant="subtle" :ui="{
                container: 'gap-y-2',
                wrapper: 'items-start',
                leading: 'bg-primary/10 ring ring-inset ring-primary/25 rounded-full p-2.5',
                title: 'text-lg font-semibold uppercase'
            }" class="rounded-lg">
            <div class="flex flex-col gap-2">
                <p class="text-base font-normal text-muted">{{ tool.description }}</p>
                <UButton :trailing-icon="`${tool.development ? 'i-lucide-construction' : 'i-lucide-arrow-right'}`"
                    :label="`${tool.development ? 'Under Development' : 'Open Tool'}`" variant="soft" block
                    :disabled="tool.development" />
            </div>
        </UPageCard>
    </UPageGrid>
</template>