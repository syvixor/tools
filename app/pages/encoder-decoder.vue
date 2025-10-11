<script lang="ts" setup>
import { Base64 } from "js-base64";

useSeoMeta({
    title: "Developer Tools - Encoder / Decoder",
    description: "Encode & Decode text using Base64.",
    ogTitle: "Developer Tools - Encoder / Decoder",
    ogDescription: "Encode & Decode text using Base64.",
    ogImage: "/og.png",
    twitterCard: "summary_large_image",
    twitterTitle: "Developer Tools - Encoder / Decoder",
    twitterDescription: "Encode & Decode text using Base64.",
    twitterImage: "/og.png",
    twitterCreator: "@syvixor",
    robots: "index, follow",
    author: "Syvixor"
});

const toast = useToast();
const { copy, copied } = useClipboard();

const value = ref("");
const isProcessed = ref(false);

const encodeBase64 = () => {
    isProcessed.value = true
    value.value = Base64.encode(value.value);
    toast.add({
        icon: "i-lucide-check",
        title: "Encoded!",
        description: "Successfully encoded.",
        color: "success",
        duration: 2000
    });
}

const decodeBase64 = () => {
    try {
        isProcessed.value = true
        value.value = Base64.decode(value.value);
        toast.add({
            icon: "i-lucide-check",
            title: "Decoded!",
            description: "Successfully decoded.",
            color: "success",
            duration: 2000
        });
    } catch (e) {
        isProcessed.value = false
        toast.add({
            icon: "i-lucide-x",
            title: "Error!",
            description: "Invalid Base64.",
            color: "error",
            duration: 2000
        });
    }
}

const handleCopy = async () => {
    await copy(value.value).then(() => {
        toast.add({
            icon: "i-lucide-check",
            title: "Copied!",
            description: "Successfully copied.",
            color: "success",
            duration: 2000
        });
    });
}
</script>

<template>
    <div class="flex flex-col justify-center md:w-4/5 gap-4 mx-auto p-4">
        <div class="block">
            <Back />
        </div>
        <div class="flex flex-col">
            <h2 class="text-xl font-bold">Encode / Decode</h2>
            <p class="text-base font-normal">Encode & Decode text using Base64.</p>
        </div>
        <div class="flex flex-col gap-2">
            <UInput type="text" placeholder="Value..." variant="soft" size="xl" v-model="value"
                :ui="{ trailing: 'pr-2' }">
                <template v-if="isProcessed && value.length > 0" #trailing>
                    <UTooltip text="Copy" :content="{ side: 'right' }">
                        <UButton :icon="copied ? 'i-lucide-copy-check' : 'i-lucide-copy'" aria-label="Copy"
                            :color="copied ? 'success' : 'neutral'" variant="ghost" size="sm" :disabled="copied"
                            @click="handleCopy" />
                    </UTooltip>
                </template>
            </UInput>
            <div class="grid grid-cols-2 gap-2">
                <UButton icon="i-lucide-sparkles" label="Encode" variant="soft" size="lg" block
                    :disabled="value.length < 1" @click="encodeBase64" />
                <UButton icon="i-lucide-sparkles" label="Decode" variant="soft" size="lg" block
                    :disabled="value.length < 1" @click="decodeBase64" />
            </div>
        </div>
        <UPageCard title="About Base64 Encoding" variant="subtle" spotlight>
            <template #description>
                Base64 is a binary-to-text encoding scheme used to safely encode data for transmission 
                or storage. It's commonly used in web development to embed images, files, or data inside 
                HTML, CSS, or URLs. This tool lets you easily encode or decode Base64 strings directly in 
                your browser, without sending your data to a server.
            </template>
        </UPageCard>

    </div>
</template>