<script setup>
import Swal from "sweetalert2";
import { computed, onMounted, ref } from "vue";
import TodoForm from "./components/TodoForm.vue";
import TodoList from "./components/TodoList.vue";

const nameValue = ref(""); // nameHandel is a reactive variable
const data = localStorage.getItem("todoList")
  ? JSON.parse(localStorage.getItem("todoList"))
  : [];
const todoLists = ref(data); // todoList is a reactive variable
const todoData = ref({
  title: null,
  category: null,
}); // todoList is a reactive variable

// const revTodoList = computed(() => todoLists.value.sort((a,b) =>{
// 	return b.id - a.id
// }))

function nameDataHandel() {
  localStorage.setItem("name", nameValue.value);
}

function handelFormSubmit() {
  if (!todoData.value.title || !todoData.value.category) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "Please fill all the fields!",
    });
    todoData.value = {
      title: null,
      category: null,
    };
    return;
  }
  todoLists.value.push({
    id: Math.max(...todoLists.value.map((todo) => todo.id), 0) + 1,
    title: todoData.value.title,
    category: todoData.value.category,
    isDone: false,
  });
  todoData.value = {
    title: null,
    category: null,
  };
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

function handelDelete(id) {
  todoLists.value = todoLists.value.filter((todo) => todo.id !== id);
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

function handelDone(id) {
  const index = todoLists.value.findIndex((todo) => todo.id === id);
  todoLists.value[index].isDone = !todoLists.value[index].isDone;
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

onMounted(() => {
  document.getElementById("name").value = localStorage.getItem("name") || "";
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
        <input
          type="text"
          id="name"
          placeholder="Name here"
          v-model="nameValue"
          @keyup="nameDataHandel"
        />
      </h2>
    </section>

    <TodoForm 
		:todoData="todoData"
		@handelFormSubmit="handelFormSubmit"
		/>

		<TodoList :todoLists="todoLists" @handelDelete="handelDelete" @handelDone="handelDone" />
  </main>
</template>

<style scoped>
</style>
