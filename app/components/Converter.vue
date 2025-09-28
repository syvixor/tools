<script lang="ts" setup>
import type { SelectItem } from "@nuxt/ui";

const file = ref<File>();

const format = ref("image/jpeg");

const formats = ref<SelectItem[]>([
    { id: "image/jpeg", label: "JPEG" },
    { id: "image/png", label: "PNG" },
    { id: "image/webp", label: "WEBP" }
]);

const convertedURL = ref();
const isProcessing = ref(false);

const handleImageConverter = async () => {
    if (!file.value || !format.value) return

    try {
        isProcessing.value = true

        const img = new Image();
        img.src = URL.createObjectURL(file.value);

        await new Promise((resolve) => img.onload = resolve);

        const canvas = document.createElement("canvas");
        canvas.width = img.width
        canvas.height = img.height

        const ctx = canvas.getContext("2d");
        if (!ctx) return;

        ctx.drawImage(img, 0, 0);

        canvas.toBlob((blob) => {
            if (blob) {
                const url = URL.createObjectURL(blob);
                convertedURL.value = url

                const link = document.createElement("a");
                link.href = url
                link.download = `converted-image.${format.value.split("/")[1]}`;
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                URL.revokeObjectURL(url);
            }
        }, format.value);
    } catch (error) {
        console.error("Error Converting Image!");
        console.error(error);
    } finally {
        isProcessing.value = false
    }
}
</script>

<template>
    <div class="space-y-4 mt-4">
        <div class="flex flex-col">
            <h2 class="text-xl font-bold">Image Converter</h2>
            <p class="text-base font-normal">Convert images between formats quickly and easily.</p>
        </div>
        <UFileUpload label="Drop Image Here" description="PNG, JPEG or WEBP" layout="list" position="outside"
            v-model="file" />
        <USelect :items="formats" v-model="format" value-key="id" variant="soft" class="w-full" :disabled="!file" />
        <UButton icon="i-lucide-sparkles" :label="isProcessing ? 'Processing...' : 'Convert Image'"
            :color="isProcessing ? 'neutral' : 'primary'" variant="soft" size="lg" block :disabled="!file"
            :loading="isProcessing" @click="handleImageConverter" />
    </div>
</template>