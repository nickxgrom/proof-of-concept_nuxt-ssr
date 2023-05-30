<template>
    <h1>List of documents</h1>

    <div
        v-for="(document, index) in documents"
        :key="document"
        class="document-card"
        @click="openDocument(document.id)"
    >
        <div class="card-title">
            <div>{{ index+1 }}. {{ document.title }}</div>
            <div
                v-if="(index+1)%2 === 0 && (index+1)%3 === 0"
                class="chip-demo"
            >
                demo
            </div>
        </div>
        <div class="card-description">{{ document.body }}</div>
    </div>
</template>

<script setup>
import { ref } from "vue"
import { useRouter } from "vue-router"
import {useFetch} from "#app";
import {useHead} from "unhead";

const documents = ref([])
const router = useRouter()

useHead({
    title: "Document list"
})

documents.value = (await useFetch("https://jsonplaceholder.typicode.com/posts")).data.value

function openDocument(id) {
    router.push(`documents/${id}`)
}
</script>

<style scoped>
.document-card {
    margin-bottom: 4px;
    padding: 8px 16px;
    background: #FFF;
    cursor: pointer;
}

.card-title {
    font-size: 1.2rem;
    font-weight: 800;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
}

.card-description {
    font-size: .8rem;
}

.chip-demo {
    margin-left: 12px;
    padding: 4px 8px;
    border-radius: 8px;
    color: #b91c1c;
    background: #fee2e2;
    font-size: 1rem;
}
</style>