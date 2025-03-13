<template>
  <div>
    <h1>Hello home page</h1>

    <AutoComponent
      text="hello dude"
      :clickme="
        () => {
          console.log('object')
        }
      "
    />

    <hr />

    <!-- Do example here -->

    <button @click="goDetail('123')">go about page</button>
  </div>
</template>

<script setup lang="ts">
import AutoComponent from '@/components/AutoComponent.vue'
import { useCounterStore } from '@/stores/counter'
import axios from 'axios'
import { onMounted, reactive, ref } from 'vue'
import { useRouter } from 'vue-router'

// ---------------------------------------------------------------------------------

onMounted(() => {
  // Same like use effect
  // Call api hear
})

// --------------------------- State management Pinia ---------------------------

const { ENDPOINT } = useCounterStore()

// --------------------------- Router ---------------------------

const router = useRouter()

// --------------------------- State ---------------------------

const isLoading = ref(false)
// use isLoading.value = true

const state = reactive<{ todo: Array<unknown> }>({
  todo: [],
})
// use state.todo = []

// --------------------------- Function ---------------------------

const fetchTodoList = async () => {
  const { data } = await axios.get<Array<unknown>>('endpoint')

  state.todo = data
}

const goDetail = (id: string) => {
  router.push(`/about/${id}`)
}
</script>

<style scoped></style>
