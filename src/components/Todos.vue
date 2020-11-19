<template>
  <div>
    <h1 class="header">Todo List</h1>
    <el-input
      placeholder="Input What Needs To Be Done"
      class="input"
      v-model="inputTodo"
      v-on:keyup.enter.native="addTodo"
    ></el-input>
    <div v-for="(todo, index) in todos" v-bind:key="todo.id" class="todos">
      <div class="todoItem">
        <el-checkbox v-model="todo.completed"></el-checkbox>
        <div
          v-if="!todo.editing"
          class="todoLabel"
          v-on:dblclick="editing(todo)"
          v-bind:class="{ completed: todo.completed }"
        >
          {{ todo.title }}
        </div>
        <input
          v-else
          class="todoEdit"
          v-model="todo.title"
          v-on:blur="edited(todo)"
          v-on:keyup.enter="edited(todo)"
          v-focus
        />
      </div>
      <div class="remove" v-on:click="deleteTodo(index)">
        <i class="el-icon-delete"></i>
      </div>
    </div>
    <hr />
    <el-checkbox v-model="checked" v-on:change="selectAll">Select All</el-checkbox>
    <br />
    <br />
    <el-button type="danger" v-on:click="clearSelected"
      >Clear Completed</el-button
    >
  </div>
</template>

<script>
export default {
  name: "todos",
  data() {
    return {
      inputTodo: "",
      id: 5,
      checked: false,
      todos: [
        {
          id: 1,
          title: "Add a new todo",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Edit a todo by double clicking on it",
          completed: false,
          editing: false,
        },
        {
          id: 3,
          title: "Remove a todo by clicking the trash icon",
          completed: false,
          editing: false,
        },
        {
          id: 4,
          title: "Remove all todos by selecting all",
          completed: false,
          editing: false,
        },
      ],
    };
  },

  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      },
    },
  },

  methods: {
    addTodo() {
      if (this.inputTodo.length == 0) {
        return alert("Enter a Todo to Add");
      }
      this.todos.push({
        id: this.id,
        title: this.inputTodo,
        completed: false,
      });
      this.inputTodo = "";
      this.id++;
    },

    deleteTodo(index) {
      this.todos.splice(index, 1);
    },

    editing(todo) {
      todo.editing = true;
    },

    edited(todo) {
      todo.editing = false;
    },

    selectAll() {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
    },

    clearSelected() {
      this.todos = this.todos.filter((todo) => !todo.completed);
      this.checked = false
    },
  },
};
</script>

<style lang="scss">
.header {
  text-align: center;
}

.input {
  margin-bottom: 16px;
  font-size: 24px;
}

.todos {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: red;
  }
}

.todoItem {
  display: flex;
  align-items: center;
}

.todoLabel {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todoEdit {
  font-size: 24px;
  color: black;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid black;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: red;
}
</style>
