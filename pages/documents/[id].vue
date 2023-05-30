<template>
    <Head>
        <Title>{{ `${isDemo && !token ? 'Demo. ' : ''}Document #${route.params.id}` }}</Title>
    </Head>
    <div v-if="!token">
        <input
            v-model="login"
            type="text"
            placeholder="Login"
        >

        <input
            v-model="password"
            type="text"
            placeholder="Password"
        >
        <input @click="auth" type="button" value="login">
    </div>
    <input
        v-else
        @click="logout"
        type="button"
        value="logout"
    >

    <h1>Document #{{ route.params.id }}</h1>
    <div
        v-if="isDemo && !token"
        class="chip-demo"
    >
        Demo
    </div>

    <p
        v-for="(p, index) in content.value"
        :key="p"
    >
        <b>{{ index + 1 }}</b>. {{ p }}
    </p>

    <div class="end-of-doc">
        {{ !token && isDemo ? "It's demo. Please login" : "End of document" }}
    </div>
</template>

<script setup async>
import { ref } from "vue"
import { useRoute } from 'vue-router'
import {useHead, useSeoMeta} from "unhead"
import {useCookie, useFetch, useRequestEvent, useRequestHeaders} from "#app"
import requestIp from "request-ip"

const route = useRoute()

const content = ref([])
const userForMeta = ref('')

const login = ref('')
const password = ref('')

const isDemo = route.params.id % 2 === 0 && route.params.id % 3 === 0
const token = useCookie("token")
userForMeta.value = (await useFetch(`https://jsonplaceholder.typicode.com/users/${route.params.id%10+1}`)).data.value

useHead({
    bodyAttrs: {
        class: "some-class"
    }
})

useSeoMeta({
    description: userForMeta.value.name,
    email: userForMeta.value.email
})

await useFetch('http://localhost:3080/test', {
    credentials: 'include',
    headers: {
        'Cookie': `test-cookie=some-cookie-value`
    }
})

// try {
//     const event = useRequestEvent()
//     console.log(requestIp.getClientIp(event.node.req))
// } catch (err) {}


await getDocument()

async function auth() {
    await useFetch('http://localhost:3080/auth', {
        credentials: "include",
        method: "POST",
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            login: login.value,
            password: password.value
        })
    })
    token.value = useCookie("token").value

    await getDocument()
}

function logout() {
    token.value = null
    window.location.reload()
}

async function getDocument() {
    let clientIp
    try {
        const event = useRequestEvent()
        clientIp = requestIp.getClientIp(event.node.req)?.split("f:")[1]
    } catch (err) {}


    const { data } = await useFetch(`http://localhost:3080/document?isDemo=${isDemo}`, {
        credentials: "include",
        headers: {
            Cookie: `token=${token.value}`,
            'client-ip': clientIp
        }
    })

    content.value = data
}
</script>

<style scoped>
h1 {
    width: fit-content;
}

.chip-demo {
    width: fit-content;
    padding: 4px 8px;
    border-radius: 8px;
    color: #b91c1c;
    background: #fee2e2;
    font-size: 1rem;
}

.end-of-doc {
    width: fit-content;
    padding: 16px;
    margin-bottom: 20px;
    font-size: 1.3rem;
    font-weight: 700;
    background: #e2e2e2;
    border-radius: 8px;
}
</style>