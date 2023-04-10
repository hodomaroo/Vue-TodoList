<!-- 할 일 정보를 입력받는 컴포넌트 -->
<template>
  <div class="inputBox shadow">
    <input
      type="text"
      maxlength="15"
      v-model="newTodoItem"
      placeholder="오늘 할 일을 적어볼까요?"
      v-on:keyup.enter="addTodo"
    />
    <span
      class="container"
      @click="addTodo"
      @mouseover="isHoveringAdd = true"
      @mouseleave="isHoveringAdd = false"
    >
      <i
        class="fa-solid fa-plus fa-xl"
        :class="{ 'fa-bounce': isHoveringAdd }"
      ></i
    ></span>

    <modal v-if="showModal" @close="closeModal">
      <h3 slot="header">경고</h3>
      <span slot="body"></span>
      <span slot="footer">
        할 일을 입력하세요
        <i class="fa-solid fa-xmark fa-bounce" aria-hidden="true"></i>
      </span>
    </modal>
  </div>
</template>

<script scoped>
import Modal from "./common/Modal.vue";
export default {
  components: { Modal },
  data() {
    return {
      newTodoItem: "",
      isHoveringAdd: false,
      showModal: false
    };
  },

  methods: {
    addTodo: function(event) {
      if (this.newTodoItem == "") {
        this.showModal = !this.showModal;
        return;
      }

      this.$emit("addTodo", this.newTodoItem);
      this.clearInput();
    },

    clearInput: function() {
      this.newTodoItem = "";
    },
    closeModal: function() {
      this.showModal = false;
    }
  }
};
</script>

<style scoped>
input:focus {
  outline: none;
}

.inputBox {
  background: whitesmoke;
  height: 50px;
  line-height: 50px;
  border-radius: 10px;
}

.inputBox input {
  border-style: none;
  font-size: 0.9rem;
  background: whitesmoke;
}

.container {
  float: right;
  background: linear-gradient(to bottom, black, gray);
  display: block;
  width: 3rem;
  border-radius: 0 10px 10px 0;
}

.addBtn {
  color: white;
  vertical-align: middle;
}

.container:hover {
  background: linear-gradient(to top, green, yellow);
  cursor: pointer;
}
</style>
