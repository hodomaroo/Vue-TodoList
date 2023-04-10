<!-- 최상위 컴포넌트! -->
<template>
  <div id="app">
    <todoHeader></todoHeader>
    <todoInput v-on:addTodo="addTodo"></todoInput>
    <todoList
      :todoItemList="todoList"
      @deleteTodo="deleteTodo"
      @convertTodo="convertTodo"
      @changeTodo="changeTodo"
    ></todoList>
    <todoFooter @clearTodo="clearTodo"></todoFooter>
  </div>
</template>

<script>
import { range, thresholdScott } from "d3-array";
import TodoFooterVue from "./components/TodoFooter.vue";
import TodoHeaderVue from "./components/TodoHeader.vue";
import TodoInputVue from "./components/TodoInput.vue";
import TodoListVue from "./components/TodoList.vue";

export default {
  data() {
    return {
      todoList: [],
      id: 0
    };
  },
  components: {
    todoFooter: TodoFooterVue,
    todoHeader: TodoHeaderVue,
    todoInput: TodoInputVue,
    todoList: TodoListVue
  },

  created() {
    if (localStorage.getItem("currentId") == null)
      localStorage.setItem("currentId", 0);
    this.id = JSON.parse(localStorage.getItem("currentId"));

    this.todoList = [...range(localStorage.length)]
      .map(idx => localStorage.key(idx))
      .filter(v => v != "loglevel:webpack-dev-server" && v != "currentId")
      .map(function(v, index) {
        return JSON.parse(localStorage.getItem(v));
      })
      .sort((a, b) => a.id - b.id);
  },

  methods: {
    addTodo: function(todoItemName) {
      localStorage.setItem(
        this.id,
        JSON.stringify({
          id: this.id,
          name: todoItemName,
          isChecked: false
        })
      );
      this.todoList.push({
        id: this.id,
        name: todoItemName,
        isChecked: false
      });

      this.id += 1;
      localStorage.setItem("currentId", this.id);
    },

    deleteTodo: function(todoItem, index) {
      localStorage.removeItem(todoItem.id);
      this.todoList.splice(index, 1);

      console.log(index);
    },

    clearTodo: function() {
      localStorage.clear();
      this.todoList = [];
    },

    convertTodo: function(todoItem, index) {
      origin = JSON.parse(localStorage.getItem(todoItem.id));
      origin.isChecked = !origin.isChecked;

      localStorage.setItem(origin.id, JSON.stringify(origin));
      this.$set(this.todoList, index, origin);
    },

    changeTodo: function(todoItem, index) {
      origin = JSON.parse(localStorage.getItem(todoItem.id));

      origin.name = todoItem.name;
      localStorage.setItem(todoItem.id, JSON.stringify(origin));
      this.$set(this.todoList, index, origin);
      console.log("finish!");
    }
  }
};
</script>

<style>
body {
  text-align: center;
  background-color: aliceblue;
  width: 50%;
  margin: 0 auto;
}
input {
  border-style: groove;
  width: 200px;
}

button {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
