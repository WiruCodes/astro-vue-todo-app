<template>
  <div class="todo-app">
    <h1>Vue Todo List</h1>

    <!-- Add todo section -->
    <div id="add-todo-container">
      <Input
        name="add-todo"
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="Add a new task..."
      />
      <Button @click="addTodo">Add</Button>
    </div>

    <!-- Todo show status choice -->
    <RadioGroup id="todo-show-status-container" v-model="showTodoStatus">
      <div v-for="o in showTodoChoicesArr" :key="o.value">
        <Label>
          <RadioGroupItem
            name="show-todo-status"
            type="radio"
            :value="o.value"
          />
          {{ o.label }}
        </Label>
      </div>
    </RadioGroup>

    <!-- TODO, add responsive grid system and fluid elements -->
    <!-- Todo item list -->
    <div class="grid gap-4 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5">
      <TodoItem
        v-for="item in filteredTodos"
        :key="item.id"
        :item="item"
        @remove="removeTodo"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import Button from "./ui/button/Button.vue";
import Input from "./ui/input/Input.vue";
import RadioGroup from "./ui/radio-group/RadioGroup.vue";
import RadioGroupItem from "./ui/radio-group/RadioGroupItem.vue";
import Label from "./ui/label/Label.vue";

import { computed, ref } from "vue";
import type { Todo } from "./types";
import TodoItem from "./TodoItem.vue";

const newTodo = ref("");
const todos = ref<Todo[]>([]);
const showTodoChoicesArr = [
  { value: "show-all", label: "show all todos" },
  { value: "show-finished", label: "show finished todos" },
  { value: "show-unfinished", label: "show unfinished todos" },
];
const showTodoStatus = ref<"show-all" | "show-finished" | "show-unfinished">(
  "show-all"
);

const filteredTodos = computed(() =>
  todos.value.filter((todo: Todo) => {
    switch (showTodoStatus.value) {
      case "show-unfinished":
        return !todo.done;
      case "show-finished":
        return todo.done;
      default:
        return true;
    }
  })
);

function addTodo() {
  const todoText = newTodo.value.trim();
  todos.value.push({ id: Date.now(), text: todoText, done: false });
  newTodo.value = "";
}

function removeTodo(id: number) {
  console.log(todos.value.find((x) => x.id === id));
  todos.value = todos.value.filter((x) => x.id !== id);
}
</script>

<style></style>
