<script lang="ts" setup>
const isReady = ref(false);
onMounted(() => (isReady.value = true));
</script>

<template>
    <div class="space-y-8" v-show="isReady">
        <Header />
        <slot />
        <Footer />
    </div>
    <div v-show="!isReady" class="flex justify-center items-center h-screen">
        <UButton aria-label="Loading" color="neutral" variant="link" size="xl" loading />
    </div>
</template>