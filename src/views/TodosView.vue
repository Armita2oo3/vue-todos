<script setup>
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";
import TodoCreator from "../components/TodoCreator.vue";
import TodoItems from "../components/TodoItems.vue";
const todoList = ref([]);
watch(
  todoList,
  () => {
    setTodoListLocalStorage();
  },
  {
    deep: true,
  }
);
const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};
const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};
fetchTodoList();
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
};
const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};
const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};
const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};
const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItems
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon
        icon="icon-park-solid:add-one"
        color="green"
        width="22"
        height="22"
      />
      <span>You have no todo's to Complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto:partying-face" color="green" width="22" height="22" />
      <span>You have Completed all of your Todos!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
h1 {
  text-align: center;
  margin-bottom: 16px;
}
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
}
.todo-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  margin-top: 24px;
  gap: 24px;
}
.todos-msg {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 24px;
  align-items: center;
}
</style>
