<!-- 하단 버튼 화면 구성 -->
<template>
  <div class="clearTodoContainer">
    <span class="clearTodo" @click="openClearConfirmModal">Clear All</span>

    <modal class="confirmModal" v-if="showModal" @close="closeModal">
      <h3 slot="header">경고</h3>
      <span slot="body">모든 TodoList를 삭제할까요?</span>
      <div class="confirm" slot="footer" @click="clearTodo">확인</div>
    </modal>
  </div>
</template>

<script>
import Modal from "./common/Modal.vue";
export default {
  components: {
    Modal
  },

  data() {
    return {
      showModal: false
    };
  },

  methods: {
    openClearConfirmModal: function() {
      this.showModal = true;
    },
    closeModal: function() {
      this.showModal = false;
    },

    clearTodo: function() {
      this.$emit("clearTodo");
      this.closeModal();
    }
  }
};
</script>

<style scoped>
.clearTodoContainer {
  line-height: 40px;
  height: 40px;
  border-radius: 5px;
  border: none;
  width: 8.5rem;
  background: black;
  margin: 0 auto;
}

.clearTodo {
  border: none;
  display: block;
  color: aliceblue;
}

.clearTodoContainer:hover {
  background: gray;
  cursor: pointer;
}

.confirm {
  background: black;
  border-radius: 10px;
  color: aliceblue;
  font-weight: bold;
}

.confirm:hover {
  background: gray;
}
</style>
