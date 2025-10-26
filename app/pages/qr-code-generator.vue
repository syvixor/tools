<script lang="ts" setup>
import { QrcodeSvg } from "qrcode.vue";

useSeoMeta({
    title: "Developer Tools - QR Code Generator",
    description: "Generate QR codes for links, text, or any data.",
    ogTitle: "Developer Tools - QR Code Generator",
    ogDescription: "Generate QR codes for links, text, or any data.",
    twitterTitle: "Developer Tools - QR Code Generator",
    twitterDescription: "Generate QR codes for links, text, or any data."
});

const toast = useToast();

const value = ref<string | undefined>(undefined);
const reference = ref<HTMLElement | null>(null);

const isModalOpen = ref(false);

const handleQRCodeGeneration = () => {
    if (value.value && value.value?.length < 200) {
        isModalOpen.value = true
        toast.add({
            icon: "i-lucide-check",
            title: "Generated!",
            description: "QR Code successfully generated.",
            color: "success",
            duration: 2000
        });
    } else {
        toast.add({
            icon: "i-lucide-x",
            title: "Too Long!",
            description: "The provided value exceeds the maximum capacity for a QR code.",
            color: "error",
            duration: 2000
        });
    }
}

const handleDownload = async () => {
    if (!reference.value || !value.value) return

    await nextTick();

    const element = reference.value.querySelector("svg");

    if (!element) return

    const svg2string = new XMLSerializer().serializeToString(element);
    const url = `data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg2string)}`;

    const link = document.createElement("a");
    link.href = url;
    link.download = "qrcode.svg";

    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
}
</script>

<template>
    <div class="flex flex-col justify-center gap-4 p-4 md:p-8">
        <div class="block">
            <Back />
        </div>
        <div class="flex flex-col">
            <h2 class="text-xl font-bold">QR Code Generator</h2>
            <p class="text-base font-normal">Generate QR codes for links, text, or any data.</p>
        </div>
        <div class="flex flex-col gap-4">
            <UTextarea type="text" placeholder="Value..." variant="soft" size="xl" autoresize v-model="value" />
            <UButton icon="i-lucide-sparkles" label="Generate QR Code" variant="soft" size="lg" block :disabled="!value"
                @click="handleQRCodeGeneration" />
            <UPageCard title="About QR Code Generation" variant="subtle" spotlight>
                <template #description>
                    QR codes are 2D barcodes capable of storing URLs, text, and other data for
                    quick scanning. Use this tool to instantly generate custom QR codes for any
                    purpose.
                </template>
            </UPageCard>
        </div>
    </div>

    <UModal title="Generated QR Code" description="QR Code has been successfully generated." v-model:open="isModalOpen">
        <template #body>
            <div class="flex justify-center items-center py-8" ref="reference">
                <QrcodeSvg :value="value" :size="200" level="H" />
            </div>
            <UButton label="Download Code" variant="soft" block @click="handleDownload" />
        </template>
    </UModal>
</template>