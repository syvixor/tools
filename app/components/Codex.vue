<script lang="ts" setup>
import { Base64 } from "js-base64";

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
            duration: 2000
        });
    } catch (e) {
        isProcessed.value = false
        toast.add({
            icon: "i-lucide-x",
            title: "Error!",
            description: "Invalid Base64.",
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
            duration: 2000
        });
    });
}
</script>

<template>
    <div class="space-y-4 mt-4">
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
    </div>
</template>