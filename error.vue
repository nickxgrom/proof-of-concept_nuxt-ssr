<template>
    <div class="container">
        <h1>Just error page</h1>
        Status code:
        <span class="chip-status">
        {{ error.statusCode }}
    </span>
    </div>
</template>

<script setup>
import {useError} from "#app"

const error = useError()
</script>

<style scoped>
.chip-status {
    margin-left: 12px;
    padding: 4px 8px;
    border-radius: 8px;
    color: #b91c1c;
    background: #fee2e2;
    font-size: 1.5rem;
}

.container {
    text-align: center;
}
</style>