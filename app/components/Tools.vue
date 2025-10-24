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
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 p-4">
        <div spotlight v-for="(tool, index) in tools" :key="index"
            class="bg-elevated/75 ring ring-accented/75 rounded-lg p-4">
            <div class="flex flex-col h-full gap-4">
                <div class="flex bg-primary/15 ring-2 ring-primary/50 w-fit rounded-full p-2">
                    <UIcon :name="tool.icon" class="bg-primary w-5 h-5" />
                </div>
                <!-- <CHANGE> added flex-1 to make content grow and push button down -->
                <div class="flex flex-col gap-4 flex-1">
                    <div class="flex flex-col">
                        <h2 class="text-xl font-bold">{{ tool.title }}</h2>
                        <p class="text-base font-normal text-muted">{{ tool.description }}</p>
                    </div>
                </div>
                <UButton :to="tool.to"
                    :trailing-icon="`${tool.development ? 'i-lucide-construction' : 'i-lucide-arrow-right'}`"
                    :label="`${tool.development ? 'Under Development' : 'Open Tool'}`"
                    :color="`${tool.development ? 'error' : 'primary'}`" variant="soft" block
                    :disabled="tool.development" />
            </div>
        </div>
    </div>
</template>