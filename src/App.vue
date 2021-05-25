<template>
  <h1>ToDo App</h1>
  <div class="card">
    <form @submit.prevent="addTodo()">
      <label>New ToDo </label>
      <input
        v-model="newTodo"
        name="newTodo"
        autocomplete="off"
        placeholder="write a todo"
      />
      <button>Add ToDo</button>
    </form>
    <h2>ToDo List</h2>
    <ul>
      <li v-for="(todo, i) in todos" :key="i">
        <input type="checkbox" id="drawline" />
        <label for="drawline" class="todo" @click="doneTodo(todo)">{{
          todo.content
        }}</label>

        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "TodoList",
  setup() {
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Write a todo",
      },
    ];
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = "";
      }
      saveData();
    }

    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>
<style lang="scss">
#app {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-content: center;
}
h1 {
  text-align: center;
  color: #333;
}
.card {
  border: 1px solid black;
  width: 33.33%;
}
//input checkbox
input[type="checkbox"] {
  display: flex;
  flex-direction: row;
  //checkbox in checked state
  &:checked {
    ~ .todo {
      color: gray;
      text-decoration: line-through;
    }
  }
}
</style>

