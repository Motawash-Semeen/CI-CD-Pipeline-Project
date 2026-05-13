<script setup>
import { defineEmits, defineProps, ref, watch } from "vue";

const props = defineProps(["todoData"]);
const todoData = ref(props.todoData);
const emit = defineEmits(["handelFormSubmit"]);
function handelFormSubmit() {
  emit("handelFormSubmit");
}
watch(() => props.todoData, (newVal) => {
  todoData.value = newVal;
});
</script>

<template>
  <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="handelFormSubmit">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="e.g. make a video"
          v-model="todoData.title"
        />

        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="todoData.category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              id="category2"
              value="personal"
              v-model="todoData.category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>
</template>