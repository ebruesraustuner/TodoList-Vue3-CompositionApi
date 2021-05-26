<template>
  <h1>ToDo App</h1>
  <div class="card">
    <div class="card__header">
      <form @submit.prevent="addTodo()">
        <label>Add</label>
        <input
          v-model="newTodo"
          name="newTodo"
          autocomplete="off"
          placeholder="write a todo"
        />
        <button class="btn btn-default">Add ToDo</button>
      </form>
    </div>
    <div class="card__main">
      <h2>ToDo List</h2>
      <hr class="break-box" />
      <ul class="card__main__list">
        <li v-for="(todo, index) in todos" :key="index">
          <span @click="doneTodo(todo)"> <input type="checkbox" id="drawline" /></span>
          <label for="drawline" :class="{ completed: todo.done }" >{{
            todo.content
          }}</label>

          <button @click="removeTodo(index)" class="btn btn-warning">
            Remove
          </button>
        </li>
      </ul>
    </div>
    <h4 v-if="todos.length === 0">Yayy ,u don't have todos :)</h4>
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
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300&display=swap");

$pink: #fe346e;
$purple: #b21f66;
$blue: #111d5e;
$background: #ecf0f3;
$white: #fff;
$header-color: #37474f;
$text-color: #455a64;
$box-color: #d1d9e6;
$white: #fff;

#app {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-content: center;
}

body {
  font-family: "Open Sans", sans-serif;
  font-size: 18px;
  background-color: $background;
}

input {
  border: 1px solid transparent;
  padding: 8px;
  margin: 0 4px;
  border-radius: 2px;
  &:focus {
    outline: none;
    background-color: $white;
    border-color: rgba(234, 76, 137, 0.4);
    -webkit-box-shadow: 0 0 0 4px rgb(234 76 137 / 10%);
    box-shadow: 0 0 0 4px rgb(234 76 137 / 10%);
  }
}

//input checkbox
input[type="checkbox"] {
  display: flex;
  flex-direction: row;
  outline: 1px solid $box-color;
  //checkbox in checked state

}

.completed {
  color: $text-color;
      text-decoration: line-through;
}

.btn {
  color: $blue;
  padding: 0.75em;
  margin: 0.25em;
  box-shadow: 2px 2px 3px rgba($header-color, 0.2);
  position: relative;
  z-index: 3;
  background-color: $background;
  border-radius: 6px;
  z-index: 2;
  border: transparent;

  &::after {
    content: "";
    width: 100%;
    height: 100%;
    display: block;
    box-shadow: -2px -2px 3px $white;
    position: absolute;
    top: 0;
    left: 0;
    border-radius: 6px;
    z-index: -1;
  }

  &:hover{
    box-shadow: inset 2px 2px 3px rgba($header-color, 0.2);
  }
}


h1 {
  text-align: center;
  color: $blue
}

    h2 {
      width: 100%;
      margin: 8px 4px;
      position: relative;

    }
h4 {
  color: $header-color;
  padding: 0 8px;
  text-align: center;
}

hr {
  //border: 2px solid black;
  width: 100%;
  height: 4px;
  box-shadow: 2px 2px 3px rgba($header-color, 0.2);
  position: relative;
  z-index: 3;
  background-color: $background;
  border-radius: 4px;
  z-index: 2;
  border: transparent;

  &::after {
    content: "";
    width: 100%;
    height: 100%;
    display: block;
    box-shadow: -2px -2px 3px $white;
    position: absolute;
    top: 0;
    border-radius: 6px;
    z-index: -1;
  }
}


.card {
  border: 1px solid #ecf0f3;
  width: 33.3%;
  min-width: 280px;
  height: auto;
  box-shadow: 28px 28px 60px rgba($header-color, 0.2);
  color: $blue;
  position: relative;
  margin: 2% 0;
  z-index: 3;
  background-color: $background;
  border-radius: 6px;
  z-index: 2;

  &::after {
    content: "";
    width: 100%;
    height: 100%;
    display: block;
    box-shadow: -32px -32px 60px $white;
    position: absolute;
    top: 0;
    border-radius: 6px;
    z-index: -1;
  }

  &__header {
    margin: 8px;
    display: flex;
    justify-content: space-evenly;
    align-items: center;

    form {
      display: flex;
      align-items: center;
    }
  }
  &__main {
    margin: 16px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

    &__list {
      width: 100%;
      margin: 0;
      padding: 0 16px 16px;
      li {
        list-style: none;
        padding: 0 0 8px;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }
    }
  }
}
</style>

