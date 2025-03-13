<template>
  <div>
    <h1>Hello about page : {{ $route.params.id }}</h1>
    <hr />

    <button @click="goHomePage">go homepage</button>
  </div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

// --------------------------- Route ---------------------------

const route = useRoute()

const router = useRouter()

// get id in script => route.params.id

onMounted(() => {
  // call api here
})

// --------------------------- Function ---------------------------

const goHomePage = () => {
  router.push('/')
}
</script>

<style scoped></style>
