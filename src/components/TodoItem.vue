<template>
  <Card
    class="flex flex-col sm:flex-row justify-between items-start sm:items-center p-4 my-2 gap-2"
  >
    <div class="flex-1 flex items-center gap-2 min-w-0">
      <Checkbox
        name="todo-done"
        v-model="item.done"
        type="checkbox"
        class="ml-1 mr-2"
      />
      <Label
        v-if="!editable"
        @click="toggleEditable('on')"
        :class="{ 'text-decoration-line: line-through': item.done }"
        class="cursor-pointer inline-block truncate"
        >{{ item.text }}</Label
      >
      <Input
        name="todo-item"
        ref="inputRef"
        v-model="item.text"
        v-if="editable"
        @blur="toggleEditable('off')"
        @keyup.enter="toggleEditable('off')"
        class="w-full"
      />
    </div>
    <div class="">
      <Button
        @mousedown.prevent="toggleEditable('on')"
        @click="toggleEditable('on')"
        class="bg-yellow-400 ml-3 pl-3 pr-3"
      >
        edit
      </Button>
      <Button @click="remove(item.id)" class="bg-red-400 ml-3 pl-3 pr-3">
        delete
      </Button>
    </div>
  </Card>
</template>

<script setup lang="ts">
import Button from "./ui/button/Button.vue";
import Label from "./ui/label/Label.vue";
import { Card, CardTitle } from "./ui/card";
import Input from "./ui/input/Input.vue";
import Checkbox from "./ui/checkbox/Checkbox.vue";

import { ref, nextTick } from "vue";
import type { Todo } from "./types";

const props = defineProps<{ item: Todo }>();

const emit = defineEmits<{
  (e: "remove", id: number): void;
}>();

const editable = ref(false);
const inputRef = ref<{ $el: HTMLInputElement } | null>(null);

function toggleEditable(action: string) {
  editable.value =
    action === "on" ? (editable.value = true) : (editable.value = false);

  nextTick(() => {
    const editInputElement = inputRef.value?.$el;
    editInputElement?.focus();
    // inputRef.value?.select();
  });
}

function remove(id: number) {
  emit("remove", id);
}

console.log(props.item);
</script>

<style></style>
