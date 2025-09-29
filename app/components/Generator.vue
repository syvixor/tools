<script lang="ts" setup>
import { v4 as uuidv4, v7 as uuidv7 } from "uuid";
import type { TabsItem } from "@nuxt/ui";

const toast = useToast();
const { copy, copied } = useClipboard();

const uuids = reactive({ v4: "", v7: "" });

onMounted(() => {
    uuids.v4 = uuidv4();
    uuids.v7 = uuidv7();
});

const versions = [
    { label: "UUID Version 4", slot: "v4" as const },
    { label: "UUID Version 7", slot: "v7" as const }
] satisfies TabsItem[];

const handleUUIDGenerate = (version: number) => {
    if (version === 4) {
        uuids.v4 = uuidv4();
    } else {
        uuids.v7 = uuidv7();
    }
}

const handleUUIDCopy = async (version: number) => {
    const uuidToCopy = version === 4 ? uuids.v4 : uuids.v7;
    await copy(uuidToCopy).then(() => {
        toast.add({
            icon: "i-lucide-check",
            title: "Copied!",
            description: "UUID successfully copied.",
            color: "success",
            duration: 2000
        });
    });
}
</script>

<template>
    <div class="space-y-4 mt-4">
        <div class="flex flex-col">
            <h2 class="text-xl font-bold">UUID Generator</h2>
            <p class="text-base font-normal">Generate unique UUIDs instantly.</p>
        </div>
        <UTabs :items="versions" variant="pill" :ui="{ trigger: 'grow' }" class="w-full">
            <template #v4>
                <div class="flex flex-col gap-2">
                    <UInput type="text" placeholder="UUID v4" variant="soft" size="xl" v-model="uuids.v4" readonly
                        :ui="{ trailing: 'pr-2' }">
                        <template v-if="uuids.v4?.length" #trailing>
                            <UTooltip text="Copy" :content="{ side: 'right' }">
                                <UButton :icon="copied ? 'i-lucide-copy-check' : 'i-lucide-copy'" aria-label="Copy"
                                    :color="copied ? 'success' : 'neutral'" variant="ghost" size="sm" :disabled="copied"
                                    @click="() => handleUUIDCopy(4)" />
                            </UTooltip>
                        </template>
                    </UInput>
                    <UButton icon="i-lucide-sparkles" label="Generate UUID" variant="soft" size="lg" block
                        @click="() => handleUUIDGenerate(4)" />
                </div>
            </template>
            <template #v7>
                <div class="flex flex-col gap-2">
                    <UInput type="text" placeholder="UUID v7" variant="soft" size="xl" v-model="uuids.v7" readonly
                        :ui="{ trailing: 'pr-2' }">
                        <template v-if="uuids.v7?.length" #trailing>
                            <UTooltip text="Copy" :content="{ side: 'right' }">
                                <UButton :icon="copied ? 'i-lucide-copy-check' : 'i-lucide-copy'" aria-label="Copy"
                                    :color="copied ? 'success' : 'neutral'" variant="ghost" size="sm" :disabled="copied"
                                    @click="() => handleUUIDCopy(7)" />
                            </UTooltip>
                        </template>
                    </UInput>
                    <UButton icon="i-lucide-sparkles" label="Generate UUID" variant="soft" size="lg" block
                        @click="() => handleUUIDGenerate(7)" />
                </div>
            </template>
        </UTabs>
    </div>
</template>