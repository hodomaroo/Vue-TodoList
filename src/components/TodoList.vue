<!-- 실제 할일 목록을 표시하는 컴포넌트 -->
<template>
  <section>
    <transition-group name="list" tag="ul">
      <li
        v-for="(todoItem, index) in todoItemList"
        class="shadow"
        :class="{ cleared: todoItem.isChecked }"
        :key="todoItem.id"
      >
        <i
          aria-hidden="true"
          :class="{ checked: todoItem.isChecked }"
          class="checkBtn fa-solid fa-check-to-slot fa-lg"
          @click="convertTodo(todoItem, index)"
        ></i>
        <span v-if="editIndex != index" @dblclick="editIndex = index">{{
          todoItem.name
        }}</span>
        <input
          class="editBox"
          v-if="editIndex == index"
          v-model="todoItem.name"
          @keydown.enter="changeTodo(todoItem, index)"
        />
        <span
          class="removeBtn"
          @click="
            deleteTodo(todoItem, index);
            hoveringIndex = -1;
          "
          @mouseover="hoveringIndex = index"
          @mouseleave="hoveringIndex = -1"
        >
          <i
            class="fa-solid fa-trash fa-lg"
            aria-hidden="true"
            :class="{ 'fa-bounce': hoveringIndex == index }"
          ></i
        ></span>
      </li>
    </transition-group>
  </section>
</template>

<script>
export default {
  props: ["todoItemList"],

  data() {
    return {
      todoItems: this.todoItemList,
      hoveringIndex: -1,
      editIndex: -1,
      editText: ""
    };
  },

  methods: {
    deleteTodo: function(todoItem, index) {
      this.$emit("deleteTodo", todoItem, index);
    },

    convertTodo: function(todoItem, index) {
      this.$emit("convertTodo", todoItem, index);
    },

    changeTodo: function(todoItem, index) {
      this.$emit("changeTodo", todoItem, index);
      this.editIndex = -1;
    }
  }
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 0px;
  text-align: left;
}
li {
  background: rgb(192, 216, 192);
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
  display: flex;

  margin: 0.5rem 0;
  padding: 0 0.9rem;
}

.checkBtn {
  line-height: 50px !important;
  margin-right: 5px;
  color: brown;
}

.checkBtn:hover {
  cursor: pointer;
}

.removeBtn {
  height: 50px;
  /* 왼쪽 나머지 빈 공간을 마진값으로 자동 설정 */
  margin-left: auto;
}

.removeBtn:hover {
  cursor: pointer;
}

.checked {
  color: cadetblue;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s;
}

.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translate(30px, 30px);
}

li.cleared {
  background: whitesmoke;
}

.editBox {
  height: 30px;
  margin: auto 0;
  background: white;
  border: none;
}
</style>
