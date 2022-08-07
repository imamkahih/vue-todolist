<template>
  <div class="container mt-2">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input
              type="text"
              class="form-control"
              v-model="todo"
              @keyup.enter="add"
            />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
          </div>
          <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
          <br />
          <small>Total TODO : {{ totalTodo }}</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, computed, onMounted, toRefs } from "vue";
import List from "./components/List.vue";
export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });
    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });
    const totalTodo = computed(() => {
      return todos.list.length;
    });
    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };
    const deleteTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };
    const doneTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      saveToLocalStorage();
    };
    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };
    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      add,
      deleteTodo,
      doneTodo,
    };
  },
};
</script>

<style></style>
