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

const value = ref<string | undefined>(undefined);
const reference = ref<HTMLElement | null>(null);

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
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div class="flex flex-col gap-4">
                <UInput type="text" placeholder="Value..." variant="soft" size="xl" v-model="value" />
                <UPageCard title="About QR Code Generation" variant="subtle" spotlight>
                    <template #description>
                        QR codes are 2D barcodes capable of storing URLs, text, and other data for
                        quick scanning. Use this tool to instantly generate custom QR codes for any
                        purpose.
                    </template>
                </UPageCard>
            </div>
            <UCard variant="soft">
                <div class="flex justify-between items-center">
                    <h2 class="text-xl font-bold">Generated QR Code</h2>
                    <UTooltip text="Download Code">
                        <UButton icon="i-lucide-download" variant="soft" size="sm" :disabled="!value"
                            @click="handleDownload" />
                    </UTooltip>
                </div>
                <div class="flex flex-col justify-center items-center h-40" v-if="!value">
                    <UIcon name="i-lucide-qr-code" class="w-12 h-12" />
                    <p class="text-base font-normal">
                        Please enter a value to generate a QR code
                    </p>
                </div>
                <div class="flex justify-center items-center py-8" ref="reference" v-else>
                    <QrcodeSvg :value="value" :size="140" level="H" />
                </div>
            </UCard>
        </div>
    </div>
</template>