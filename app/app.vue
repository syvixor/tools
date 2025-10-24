<script lang="ts" setup>
useHead({ htmlAttrs: { lang: "en" } });

useSeoMeta({
    ogImage: "/og.png",
    twitterCard: "summary_large_image",
    twitterImage: "/og.png",
    twitterCreator: "@syvixor",
    robots: "index, follow",
    author: "Syvixor"
});
</script>

<template>
    <UApp>
        <NuxtLayout>
            <NuxtPage />
        </NuxtLayout>
    </UApp>
</template>