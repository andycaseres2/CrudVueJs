<script setup>
import { ref, onMounted, computed, watch } from "vue"

const todos = ref([])
const name = ref("")
const inputContent = ref("")
const inputCategory = ref(null)

const todosAsc = computed(() =>
  todos.sort((a, b) => {
    return a.createdAt - b.createdAt
  })
)
</script>

<template>
  <main class="w-full flex flex-col justify-center items-center my-8">
    <section class="w-full flex justify-center">
      <h2>
        What's up
        <input
          class="border-none"
          type="text"
          placeholder="Name Here"
          v-model="name"
        />
      </h2>
    </section>
  </main>
</template>
