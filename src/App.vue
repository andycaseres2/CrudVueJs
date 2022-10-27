<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");
const inputContent = ref("");
const inputCategory = ref(null);

const todosAsc = computed(() =>
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

const addTodo = () => {
  if (inputContent.value.trim() === "" || inputCategory.value === null) {
    return;
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    createdAt: new Date().getTime(),
    done: false,
  });

  inputContent.value = "";
  inputCategory.value = null;
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos") || []);
});
</script>

<template>
  <main class="w-full flex flex-col justify-center items-center">
    <div
      class="w-full border md:w-1/2 flex flex-col justify-center items-center my-8 px-6 py-8"
    >
      <section class="w-full flex pb-6">
        <h2 className="text-2xl font-semibold text-[#41B883]">
          What's up,
          <input
            class="border-none focus:outline-none ml-2 bg-transparent placeholder:text-[#41B883]"
            type="text"
            placeholder="Name Here"
            v-model="name"
          />
        </h2>
      </section>

      <section class="w-full">
        <h3 class="font-semibold text-xl text-[#41B883] mb-2">CREATE TODO</h3>
        <form @submit.prevent="addTodo">
          <h4 class="text-sm text-gray-300 font-bold mb-2">
            What's on your todo list?
          </h4>
          <input
            class="h-10 mb-2 px-2 w-full bg-white text-[#41B883] font-semibold placeholder:text-[#41B883] placeholder:font-semibold rounded"
            type="text"
            placeholder="e.g. make a video"
            v-model="inputContent"
          />
          <h4 class="text-sm font-semibold text-gray-300 mt-4 mb-2">
            Pick a category
          </h4>
          <div class="w-full flex justify-between gap-8">
            <label
              class="bg-white rounded w-full flex flex-col justify-center items-center px-14 py-5 gap-1"
            >
              <input
                type="radio"
                name="category"
                value="business"
                v-model="inputCategory"
              />
              <span></span>
              <div class="text-sm font-semibold">Business</div>
            </label>

            <label
              class="bg-white rounded w-full flex flex-col justify-center items-center px-14 py-5 gap-1"
            >
              <input
                class="accent-pink-500"
                type="radio"
                name="category"
                value="personal"
                v-model="inputCategory"
              />
              <span></span>
              <div class="text-sm font-semibold">Personal</div>
            </label>
          </div>
          <input
            class="w-full bg-[#41B883] hover:opacity-90 cursor-pointer py-2 rounded mt-4 text-white font-semibold"
            type="submit"
            value="Add Todo"
          />
        </form>
      </section>

      <section class="w-full mt-4">
        <h3 class="text-[#41B883] font-semibold mb-2 text-lg">TODO LIST</h3>

        <div class="flex flex-col gap-3">
          <div
            v-for="todo in todosAsc"
            :key="todo.createdAt"
            :class="`py-3 items-center flex rounded shadow bg-white ${
              todo.done && 'text-gray-400'
            }`"
          >
            <label class="cursor-pointer block px-4">
              <input
                type="checkbox"
                v-model="todo.done"
                :class="`rounded-full ${
                  todo.category === 'personal' && 'accent-pink-500'
                }`"
              />
              <span :class="`${todo.category}`"></span>
            </label>

            <div>
              <input
                :class="`border-none focus:outline-none ml-2 bg-transparent${
                  todo.done &&
                  'text-gray-400 line-through decoration-2 decoration-gray-400'
                }`"
                type="text"
                v-model="todo.content"
              />
            </div>

            <div class="flex w-full justify-end px-4">
              <button
                class="text-white text-sm font-semibold bg-red-500 hover:bg-red-600 px-3 py-1 rounded"
                @click="removeTodo(todo)"
              >
                Delete
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>
