<template>
  <div>
    <h1>Hello auto components : {{ text }}</h1>

    <button @click="clickme">click me</button>
  </div>
</template>

<script setup lang="ts">
type Props = {
  text: string
  clickme: () => void
}

const { text, clickme } = defineProps<Props>()

// defineProps(['text', 'clickme'])

// const props = defineProps<Props>()
// props.text
</script>

<style scoped></style>
