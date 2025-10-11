<script lang="ts" setup>
import type { SelectItem } from "@nuxt/ui";

useSeoMeta({
    title: "Developer Tools - Image Converter",
    description: "Convert images between formats quickly and easily.",
    ogTitle: "Developer Tools - Image Converter",
    ogDescription: "Convert images between formats quickly and easily.",
    ogImage: "/og.png",
    twitterCard: "summary_large_image",
    twitterTitle: "Developer Tools - Image Converter",
    twitterDescription: "Convert images between formats quickly and easily.",
    twitterImage: "/og.png",
    twitterCreator: "@syvixor",
    robots: "index, follow",
    author: "Syvixor"
});

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
    <div class="flex flex-col justify-center md:w-4/5 gap-4 mx-auto p-4">
        <div class="block">
            <Back />
        </div>
        <div class="flex flex-col">
            <h2 class="text-xl font-bold">Image Converter</h2>
            <p class="text-base font-normal">Convert images between formats quickly and easily.</p>
        </div>
        <UFileUpload label="Drop Image Here" description="PNG, JPEG or WEBP" accept="image/*" layout="list"
            position="outside" v-model="file" />
        <USelect :items="formats" v-model="format" value-key="id" variant="soft" :disabled="!file" class="w-full" />
        <UButton icon="i-lucide-sparkles" :label="isProcessing ? 'Processing...' : 'Convert Image'"
            :color="isProcessing ? 'neutral' : 'primary'" variant="soft" size="lg" block :disabled="!file"
            :loading="isProcessing" @click="handleImageConverter" />
        <UPageCard title="About Image Conversion" variant="subtle" spotlight>
            <template #description>
                This tool allows you to convert images between common formats like JPEG, PNG, and WEBP 
                directly in your browser. It works by rendering the selected image on an HTML5 canvas and 
                exporting it in the chosen format—no server upload required, ensuring your images stay 
                private and secure.
            </template>
        </UPageCard>
    </div>
</template>