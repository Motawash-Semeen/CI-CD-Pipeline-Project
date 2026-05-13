<script setup>
import { defineEmits, defineProps, ref, computed } from "vue";

const props = defineProps(["todoLists"]);
// const todoLists = ref(props.todoLists);
const revTodoList = computed(() => [...props.todoLists].reverse());

const emit = defineEmits(["handelDone", "handelDelete"]);
function handelDone(id) {
  emit("handelDone", id);
}
function handelDelete(id) {
  emit("handelDelete", id);
}
</script>

<template>
  <section class="todo-list" v-if="todoLists.length > 0">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">
        <div
          :class="{ 'todo-item': true, done: todoList.isDone }"
          v-for="todoList in revTodoList"
          :key="todoList.id"
        >
          <label>
            <input
              type="checkbox"
              :checked="todoList.isDone"
              @click="handelDone(todoList.id)"
            />
            <span
              :class="`bubble ${
                todoList.category == 'business' ? 'business' : 'personal'
              }`"
            ></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todoList.title" />
          </div>

          <div class="actions">
            <button class="delete" @click="handelDelete(todoList.id)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </section>
</template>