<script setup>
import { ref, defineEmits, reactive } from "vue";
import TodoButton from "@/components/TodoButton.vue";

const emit = defineEmits(["create-todo"]);

const todoState = reactive({
  todo: "",
  invalid: null,
  errorMsg: "",
});

const createTodo = () => {
  todoState.invalid = null;
  if (todoState.todo !== "") {
    emit("create-todo", todoState.todo);
    todoState.todo = "";

    return;
  }
  todoState.invalid = true;
  todoState.errorMsg = "Todo value cannot be empty!";
};
</script>

<template>
  <div>
    <div :class="todoState.invalid ? 'input-err' : ''" class="input-wrap">
      <input type="text" v-model="todoState.todo" />
      <TodoButton @click="createTodo()">Create Todo!</TodoButton>
    </div>
    <p v-if="todoState.invalid" class="error-msg">{{ todoState.errorMsg }}</p>
  </div>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  &.input-err {
    border-color: red;
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;
    overflow: hidden;

    &:focus {
      outline: none;
    }
  }
}
.error-msg {
  margin-top: 7px;
  color: red;
  font-size: 14px;
}
</style>
