<template>
    <h1>Hello, SSR</h1>
    <div>
        <NuxtLink to="/documents">Documents</NuxtLink>
    </div>
    <div>
        <NuxtLink to="/non-exist-path">404 test</NuxtLink>
    </div>

</template>

<script setup>
import {useHead, useSeoMeta} from "unhead";

useHead({
    title: "Hello, SSR"
})

useSeoMeta({
    description: 'some description'
})
</script>

<style scoped>

</style>