<template>
  <div class="container" :class="{red: everythingRed}">
    <div class="navigation">
      <nuxt-link to="/">Hub</nuxt-link>
      <nuxt-link to="/documents">Documents</nuxt-link>
    </div>
    <router-view/>
  </div>
</template>

<script setup>
import {onMounted, ref} from "vue"

const everythingRed = ref(false)

onMounted(() => {
  everythingRed.value = true
})
</script>

<style>
/*@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');*/

html, body {
  margin: 0;
  background: #F1F1F1;
  font-family: 'Open Sans', sans-serif;
}


.container {
  width: 80%;
  margin: 0 auto;
}

.container.red {
  /*background-color: #b91c1c;*/
}

.navigation {
  display: flex;
  gap: 20px;
  padding: 20px;
}
</style>