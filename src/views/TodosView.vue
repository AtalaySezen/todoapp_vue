<script setup>
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import TodoCreator from "@/components/TodoCreator.vue";
import TodoItem from "@/components/TodoItem.vue";

const todoList = ref([]);

watch(
  todoList,
  () => {
    saveTodoListLocalStorage();
  },
  {
    deep: true,
  }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const getTodoListFromLocalStorage = () => {
  const getTodos = JSON.parse(localStorage.getItem("todos"));

  if (getTodos) {
    todoList.value = getTodos;
  }
};

getTodoListFromLocalStorage();

const saveTodoListLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
};

const toggleTodoComplete = (id) => {
  todoList.value[id].isCompleted = !todoList.value[id].isCompleted;
};

const editTodo = (id) => {
  todoList.value[id].isEditing = !todoList.value[id].isEditing;
};

const updateTodo = (val, index) => {
  // todolistteki id ile tıkladığının indexi eşleşen todoyu bul
  // todoyu bulduktan sonra gelen val değerini bulduğun objenin valuesuna eşitleyip updatele.
  todoList.value[index].todo = val;
};

const deleteTodo = (id) => {
  todoList.value = todoList.value.filter((item) => item.id !== id);
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-bind:key="index"
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-completed="toggleTodoComplete"
        @edit-todo="editTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">You haven't any todos. Add one!</p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      You have completed all todos!
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
