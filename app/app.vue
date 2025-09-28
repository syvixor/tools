<script lang="ts" setup>
useHead({ htmlAttrs: { lang: "en" } });

const isReady = ref(false);
onMounted(() => (isReady.value = true));
</script>

<template>
    <Head>
        <Title>Tools</Title>
    </Head>
    <UApp>
        <div v-show="isReady">
            <Theme />
            <Tools />
            <Github />
        </div>
        <div v-show="!isReady" class="flex justify-center items-center h-screen">
            <UButton label="Loading" color="neutral" variant="link" size="xl" loading />
        </div>
    </UApp>
</template>